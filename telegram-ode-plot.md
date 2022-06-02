# 🤖 Telegram ODE plot

I've created a Telegram Bot that let me visualize the solution of an ODE by simply writing the equation itself.

To write the bot I've used a wrap I've developed for this purpose. You can find the package (and some details about its usage) at my [github page](https://github.com/clarkmaio/TelegramBot).

Ok but this is the boring part... let's focus on the bot itself.

### How to start a session with the bot?

This is simple: just open your telegram app and look for **OdePlot**, click on the first result and then **START**button.

At this point you will see this:

![](.gitbook/assets/screen\_1\_mod.png)

### How I am supposed to use it?

Very simple... you have just to write down an equation of a first order ode `y'=f(x,y)`.

You will immediately receive the image of the plot of the solution. For example writing `y'=y` (see screenshots below).

![](.gitbook/assets/screen\_2.png)

### Can I change settings?

Of course! you can setup 2 values:

* **ODE initial condition**: you can set the value of the solution at `x=0` (it is necessary to get a unique solution) by using the command `/set_y0`
* **x axis interval**: you can set the x axis interval that will be displayed in the plot by using the command `/set_x_interval`

You can print a recap of all commands by using the command `/help`

![](<.gitbook/assets/screen\_3 (1).png>) ![](<.gitbook/assets/screen\_4 (2).png>)

### Ok but why it's not working?

Because I am lazy.

I should set a machine (maybe a raspberry) to constantly run the python code that generate the bot... but I am lazy.

BUT of course you can try it running by your self the `OdePlotBot.py` script you will find in [**this**](https://github.com/clarkmaio/TelegramBot) repository.

