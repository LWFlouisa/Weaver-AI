---
title: Implementing A Simple Fitness Function
published: true
---

Contrary to popular belief, a fitness function isn’t just something used for neural networks, and in fact can have other application in General Intelligence, networks of multiple narrow AIs. In this post, we will implement a simple fitness function that connects different shell commands in ruby.

### Implement Shell Commands

~~~ruby
def list_files
  system(“ls -ls”)
end

def time
  system(“time”)
end

def date
  system(“date”)
end

def cal
  system(“cal”)
end

def weather
  system(“curl wttr.in”)
end
~~~

This points to different shell commands that the fitness function will point to when the user’s datapoint matches the bot’s datapoint. This datapoint will be picked from a list in numerical order, and only performs the function when it reaches that specific input value.

Now lets input a reset function:

~~~ruby
def reset_evo
  # Gets the bot shape.
  bot_shape = File.readlines("data/bot_shape/wings.txt")

  # Create a new input vector based on bot data size.
  bot_input = bot_shape.size.to_i

  # Reset user input to zero.
  reset_usr_input = 0

  open("data/number/input.txt", "w") { |f|
    f.puts reset_usr_input
  }

  # Get random bot_input from bot reset.
  bot_reset = rand(bot_input)

  open("data/bot_input/input.txt", "w") { |f|
    f.puts bot_reset
  }
end
~~~

This gets the bot shape, the makes the bot input random vector based on the size of the bot shape data set, and then resets the user’s input to zero and the bot’s input comparison to a random value less than or equal to the maximum size of the the bot’s total data set.

Now let’s implement a use_form function.

~~~ruby
def use_form
  model_type = File.read("data/model/model_type.txt").strip

  puts 'Form status: Suitable...'

  sleep(3)

  number           = File.read("data/number/input.txt").strip.to_i
  usr_wings        = File.readlines("data/usr_shape/wings.txt")
  current_wingset  = usr_wings[number]

  puts "  Model Type: #{model_type}\n  Action: #{current_wingset}"

  sleep(3)

  system("clear")

  if    current_wingset == "list_files"; list_files
  elsif current_wingset ==       "time";       time
  elsif current_wingset ==       "date";       date
  elsif current_wingset ==        "cal";        cal
  elsif current_wingset ==    "weather";    weather
  end

  # Resets the fitness function to a random bot input.
  reset_evo

  abort
end
~~~

This gets the model type that is read to the terminal, tells you that the form picked is suitable ( one of the possible shell commands in this case ) and performs a shell function based on the suitable data point determined by the user and bot dataset comparison.

Now for the reshape functionality:

~~~ruby
def reshape
  system("clear")

  model_type = File.read("data/model/model_type.txt").strip

  bot_choice = File.read("data/bot_input/input.txt").strip.to_i
  number     = File.read("data/number/input.txt").strip.to_i

  usr_wings = File.readlines("data/usr_shape/wings.txt")
  bot_wings = File.readlines("data/bot_shape/wings.txt")

  current_wingset  = usr_wings[number]
  current_botwings = bot_wings[bot_choice]

  puts "The user #{model_type} model is: #{current_wingset}"

  puts "The bot #{model_type} model is: #{current_botwings}"

  if current_wingset == current_botwings
    use_form
  else
    puts "Form status: Changing form..."

    sleep(3)

    new_value = number + 1

    open("data/number/input.txt", "w") { |f|
      f.puts new_value
    }
  end

  reshape
end
~~~

This compares the user’s dataset to the bot’s dataset, and the user only performs the datapoint that matches the bot’s randomly chosen “botwings”. In fact the main difference between this and a decision tree, is that this is based on string data rather than numerical data. It also differs from if then else in that the user isn’t picking a shell function from the aether, but comparing to an actual dataset.

I wrote this article, as to me the path to General Intelligence is not through trying to make one Machine Learning algorithm do everything, but having a fitness function pick between a smaller data set different machine learning algorithms that do individual things very well, sparing the need to collect a wide array of privacy invasive data values. One datapoint can be a naive bayes, one can be a decision tree, one can be a kmeans, one can be a neural network.

But this function would choose the function based on comparing the user’s and bot’s existing data values.

Now lets call the function:

~~~
reset_evo
reshape
~~~
