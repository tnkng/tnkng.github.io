How to install ansible with pip on RPi3 Raspbian jessie

Having purchased Raspbery Pi 3 recently, I wanted to see how far I can push the limits of this small computer and run few things for home network.

As with every new gadget, and few manual experiments, I come to (for now!) stable config of number of services I want to run, so it came to a point where I want to automate this configuration so that I can expand (more RPi3 in order queue) and replicate this configuration.

My tool of choice for automation is ansible, so I went to apt-get me an ansible.... 

For the context I'm running Raspbian jessie on the Pi, and it's really loaded with precompiled packages, but ansible is a bit old, and I wanted the latest stable ansible - 2.2.

As there was a bit trial and error, running multiple pip install commands trying to come with appropriate system lib dependencies, I reckon some folks might find this useful, and handy shortcut. 

So, in all its glory pip install and required dependencies: 

```
pi@m: sudo apt install libyaml-dev libffi-dev libgmp-dev  libssl-dev
pi@m: sudo pip install ansible
```

and the result:
```
...
Successfully installed ansible paramiko jinja2 PyYAML setuptools pycrypto cryptography pyasn1 MarkupSafe idna six enum34 ipaddress cffi pycparser
Cleaning up...
pi@m:~ $ ansible --version
ansible 2.2.0.0
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/usr/share/ansible']
pi@m:~ $
```

And simple test:
```
pi@m:~ $ ansible all -m ping
m.home.example.com | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
pi@m:~ $
```

It's not all clean though... If you pay attention to pip install logs, you might notice the following:
```
      Successfully uninstalled Jinja2
Compiling /tmp/pip-build-5BonjN/jinja2/jinja2/asyncfilters.py ...
  File "/tmp/pip-build-5BonjN/jinja2/jinja2/asyncfilters.py", line 7
    async def auto_to_seq(value):
            ^
SyntaxError: invalid syntax

Compiling /tmp/pip-build-5BonjN/jinja2/jinja2/asyncsupport.py ...
  File "/tmp/pip-build-5BonjN/jinja2/jinja2/asyncsupport.py", line 22
    async def concat_async(async_gen):
            ^
SyntaxError: invalid syntax
```

This is documented in https://github.com/pallets/jinja/issues/643 and seems like it can be worked around using upgrading pip (https://github.com/pypa/pip/issues/1873) or just ignore it!
