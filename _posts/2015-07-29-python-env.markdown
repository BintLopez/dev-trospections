What I just did to get a quick python env up and running.

On command line prompt...
Hey, don't know what the command line is? Actually, my last exposure to python, I didn't know what the command line was :-)
Stop here for this... [Explanation of command line](link-to-post)

`$ wget https://bootstrap.pypa.io/get-pip.py`

The only two `sudo` commands you'll need to run:

`$ sudo python get-pip.py`
Downloads stuff so you can use the pip command.

`$ sudo pip install virtualenv`
Uses pip to download virtualenv

 `$ mkdir virtualenv`
Makes a folder to hold your virtualenv configuration

 `$ virtualenv taxi-ticket`
result:
 '''TODO FORMAT AS CODE
	New python executable in taxi-ticket/bin/python
	Installing setuptools, pip, wheel...done.
	END'''

`cd` on over to your project directory...
`$ source bin/activate`
... to activate python inside your project
At this point your command line changes to let you know that you're in python environment land.
`(taxi-ticket)➜  taxi-ticket` is what mine looked like. I use Oh My Zsh with iterm for the command line and they show this automatically.

`pip install -r requirements.txt`
This installs your dependencies... like `bundle install` does for rails.

However... you will need a .env file
And now the Django bit. Stay posted...

(taxi-ticket)➜  taxi-ticket git:(master) deactivate