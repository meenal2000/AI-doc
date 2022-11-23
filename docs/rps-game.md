# Rock Paper Scissors Game In Python

## *Creating a python bot that plays the rock paper scissors game with the help of OOP principles in python.*

# RPS Game Bot

In this blog we will see how to make a python bot playing rock paper scissors with the help of object oriented programming in python. We will be making two files one which will contain the actual logic of the game and another file that will make a call for the bot and run it. We will be also adding some user information for the one playing the game and see how to call functions to see that info.

So without any further waiting lets get into the flow of the code. <br>
All the code for this blog is present here - [Rock-Paper-Scissors-Game](https://github.com/Hrushi11/Blogs-Repository/tree/main/Rock-Paper-Scissors-Game)

**Structure of the files :** <br>
1. [RPS_Bot.py](https://github.com/Hrushi11/Blogs-Repository/blob/main/Rock-Paper-Scissors-Game/RPS_Bot.py) - Contains the actual logic of the bot. <br>
2. [Run.py](https://github.com/Hrushi11/Blogs-Repository/blob/main/Rock-Paper-Scissors-Game/Run.py) - Contains the code to run the bot. 

## RPS Bot :

In this section we will be coding the actual logic for the bot the code for it to play the rock-paper-scissors and to get the user info. Let's begin.

**Importing Dependancies :** <br>
In case of this we need only `random` module to randomly choose between rock, paper and scissors for our bot as a choice.

```
import random
```

**Creating an User Class :** <br>

Now to begin from writing the logic for the actual code first lets work on user-info like the login status and some personal info like name, age and email. Also let's add some functionality to view all these things as profile.

```
class User:
    def __init__(self, name, age, email):
        self.age = age
        self.name = name
        self.email = email
    
    def sign_in(self):
        sgn_in = "LOGGED IN" if int(self.age) >= 3 else f"UDER-AGE, LOGGED OUT - {self.age} years"
        
        return sgn_in
    
    def profile(self):
        prf = f"Name - {self.name} \nAge - {self.age} \nEmail - {self.email}."
        
        return prf
```

`__init__()` - is a constructor like thing in python which is used to initialize the object's state, like if instantiate an object and then call the name function on it we will be getting the name. You will get a better idea of this as we go ahead.

`sign_in()` - is a self-explanatory function, this function checks the user's age and outputs accordingly.

`profile()` -  it allows the user to view all of the info in one look.

Nothing special till now, let's move ahead and now get into the actual code for the bot.

Creating a list containing rock, paper and scissors elements.

```
rps_labels = [ "rock", "paper", "scissor"]
```

### Code for the Bot :

To begin with first let's create a new class `RPS_Bot` and then inherit the properties of the `User` class in it. 

```
class RPS_Bot(User):
     def __init__(self, name, age, email, choice):
        self.age = age
        self.name = name
        self.email = email
        self.choice = choice
        self.bot_choice = "BOT CHOICE NOT INIITIALIZED YET"
``` 
Inheriting the properties of the `User` class means we can use the functions of the same like :

```
# Instantiating the object
rps1 = RPS_Bot("hrushi", "13", "hrushi@email.com", "rock")
print(rps1.sign_in())
print(rps1.profile())
```

Let's extend this and begin writing the actual logic:

```
class RPS_Bot(User):
     def __init__(self, name, age, email, choice):
        self.age = age
        self.name = name
        self.email = email
        self.choice = choice
        self.bot_choice = "BOT CHOICE NOT INIITIALIZED YET"
    
     def rps_bot(self):
         # Initialting bot choice and positions of players
         c = 1 # Check for winning
         players = [self.name, "RPS-BOT"]
         user_choice = self.choice.lower() # to keep similarities all throughout
         bot_choice = random.choice(rps_labels) 
         orig = [user_choice, bot_choice] # original order
         check = [user_choice, bot_choice] # order will be changed after sorting
         check.sort() # to sort the array
         
         # To check for draw
         if check[0] == check[1]: 
             c = 0
             res = "DRAW"
         
         # To predict winner in different situations
         if check == ['paper', 'rock']: 
             res = "paper"
         elif check == ["paper", "scissor"]:
             res = "scissor"
         elif check == ["rock", "scissor"]:
             res = "rock"
         
         # getting the position of the winner from the 'orig' array
         if c: # works if it's not a draw
             for i in range(len(orig)): 
                 if res == orig[i]: 
                     index = i # position of the winner
             res = f"{players[index]} won"     
         
         # returning winner, user and bot choice
         return res, user_choice, bot_choice 
```
The `rps_bot()` function is the one responsible for the working of the bot, it randomly generates a choice for the bot and then evaluates it with the user input. At last it returns the winner, user's choice and bot choice so that the user can also have a look on what choice the bot made and thus making it clear to him for his reason to win or lose.

**Logic of the rps_bot() :**

In the first half of the function we initiate the position `0` for the user and `1` for the bot, next we lower case the string submitted by the user with - `lower()`. The reason for doing the same is the user might not always input in our desired format and choose any format he/she likes, such as :
`rOCK`, `Rock`, `RoCk`. So the reason is to avoid this. <br>
In this same part we also initiate a random value to the our `bot_choice` variable which is our bot's choice out of `[ "rock", "paper", "scissor"]`. 

In the second half we sort the array of the input choices of both the players the user and the bot.
This is done so that we can compare it to our pre-sorted arrays for the results in specific situations like  if user chooses `rock` and bot chooses `scissors`, so the user wins. The code for this part is self-explanatory also in the same part we have condition of 'draw' when both the user and the bot chooses the same element. From this part we try to extract some result like if it's a draw or what is the element that won from `[ "rock", "paper", "scissor"]`.

In the third part we try to get the winner with the result obtained in the second part, as we have already kept the `orig` array intact which holds the elements in such a way that, 0<sup>th</sup> element is the user choice and the 1<sup>st</sup> element is bot choice. if the `orig` was ["rock", "scissors"] and the result from the second part comes out to be `"rock"` then the one with 0<sup>th</sup> position wins which is the user. Before all this happens we have a check variable `c` that remains 1 throughout the program and becomes 0 if its a draw.

Finally the function ends with returning the result, user's choice and bot's choice.

The heart of our code is done now let's move ahead and create a function that triggers this function (makes a function call for `rps_bot()`) and works only if the user is above the age of 3 and has a made a proper choice. This function would return a proper result with, who won and who chose what. let's see.

```
def rps_initiate(self):
         print("-------------------------------------------")
         if int(self.age) >= 3 and self.choice.lower() in rps_labels:
             res, user_choice, bot_choice = self.rps_bot()
             stmnt = f"{res} \n{self.name} Chose - {user_choice} \nRPS-BOT Chose - {bot_choice}"
         
         else :
             stmnt = f"YOU MIGHT BE UNDER-AGE - {self.age} years, or \nyou chose `{self.choice}`, 
                       which might NOT BE IDENTIFIED, RE-CHECK"

         return stmnt
```

This function is quite self-explanatory and so I won't explain it any further. 

### Testing the bot :

**Run this code below all this :**

```
# Intializing the RPS_Bot object
bot1 = RPS_Bot("Hrushi", "13", 
               "hrushi@email",
               "roCk")

print(bot1.rps_initiate())
```

*Output :*
```
-------------------------------------------
RPS-BOT won 
Hrushi Chose - rock 
RPS-BOT Chose - paper
```

## Running RPS Bot :

So now we are done with writing the code for the bot lets see how to use this bot in different python files. 

Create a new python file, in my case I name it `Run.py` you can choose on your own.

In this file first we have to import the all the useful classes from `RPS_Bot.py` (the name of the file that contains all the previous code).

```
from RPS_Bot import *
```  

Now let's create a `run()` function which takes input all the parameters and then initiate our bot with the same. 

```
def run():
    name = input("Enter your name \n")
    age = input("Enter your age \n")
    email = input("Enter your email \n")
    choice = input("Choose from `ROCK`, `PAPER`, `SCISSOR` \n")
    
    return name, age, email, choice

name, age, email, choice  = run()
```
Initiating the bot :

```
bot1 = RPS_Bot(name, age, 
               email, 
               choice)
print(bot1.rps_initiate())
print("-------------------------------------------")
```

When you run all the above code we will receive the output : 

```
Enter your name
Hrushi
Enter your age
13
Enter your email
hrushi@email.com
Choose from `ROCK`, `PAPER`, `SCISSOR`
rocK
-------------------------------------------
DRAW
Hrushi Chose - rock
RPS-BOT Chose - rock
-------------------------------------------
```

The `run()` function in this case can even be added to the `RPS_Bot` file so that it becomes more easier for us to use the bot, maybe this would be an exercise for you. Let me know when you do it.

We can also use other functions of the `User` class as well, since we have inherited this class.

```
# functions
print(bot1.profile())
print(bot1.sign_in())

# attributes
print(bot1.name)
print(bot1.choice)
print(bot1.bot_choice)
```

So, finally we are done with this project, all the files for this project are available here - [Rock-Paper-Scissors-Game](https://github.com/Hrushi11/Blogs-Repository/tree/main/Rock-Paper-Scissors-Game).

To know more about me you can visit - [Heights for AI](https://hrushi11.github.io/Heights-for-AI/)
