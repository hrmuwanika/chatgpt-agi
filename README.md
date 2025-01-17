# Asterisk ChatGPT intergration -  Proof of Concept

Read the blogpost about this [here](https://developer.speakup.nl/asterisk-meets-chatgpt-enhancing-telecommunications-with-ai/)

A proof of concept AGI script that integrates Asterisk with ChatGPT to hold converstations with ChatGPT.

## Deployment
#### Cloning and installing dependencies
Clone the repo somewhere on your Asterisk system. For example, to `/usr/src/`:
```bash
apt install -y git python3 python3-dev python3-pip
pip3 install --upgrade pip
ln -s /usr/bin/pip3 /usr/bin/pip

cd /usr/src/
git clone https://github.com/speakupnl/chatgpt-agi.git
```

Then, create a virtual environment and install the dependencies:

```bash
cd chatgpt-agi
pip3 install -r requirements.txt
```

Make sure to replace the API key in `chatgpt_agi.py` to your own. 

```bash
nano chatgpt_agi.py
```

#### Configuring Asterisk
Copy the `chatgpt-welcome.wav` or replace it with your own.

Please note, the actual path of your sounds directory may be different depending on your system.

```bash
cp chatgpt-welcome.wav /var/lib/asterisk/sounds/en/
cp chatgpt_agi.py /var/lib/asterisk/agi-bin/

chmod -R 755 /var/lib/asterisk/agi-bin/chatgpt_agi.py
```

Next, edit your `extensions.conf`. 

```bash
nano /etc/asterisk/extensions.conf
```

Here is an example of what the dialplan might look like. Replace the phone number to your own.

```
[internal]
exten = _2222,1,Noop(ChatGPT)
 same = n,answer()
 same = n,AGI(/var/lib/asterisk/agi-bin/chatgpt_agi.py)
 same = n,Hangup()
```

Reload the Asterisk dialplan

```bash
asterisk -rx 'dialplan reload'
```

And call away!


## Troubleshooting
If you experience any trouble, check the Asterisk console:

```bash
asterisk -rvvv
```

or check the Asterisk syslog. On systems with `systemd`:

```bash
journalctl -fu asterisk
```
