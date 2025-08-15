# StartUpSoundShuffler

Remember when computers used to greet you with a startup sound? Windows XP, Mac OS 9, and other classics had that little burst of audio that made logging in feel special. StartUpSoundShuffler brings that feeling back, but with a twist: instead of hearing the same sound every time, it shuffles through a collection of your chosen audio files so you never quite know what you will get.

If you are someone who cannot settle on just one startup sound, this program is for you. You can load it with nostalgic sounds from old systems, your favorite music clips, or even custom sound effects. The script will randomly pick one each time you log in. Of course, you can also keep only one sound if you prefer consistency.

## How it works

- Looks for audio files in `~/.sounds`  
- Ignores hidden files (anything starting with a dot)  
- Picks one at random and plays it with `ffplay`  
- Creates `~/.sounds` if it does not exist, prompting you to add sounds  
- Gives clear output if something is missing or not working  

## Requirements

`ffplay` (part of FFmpeg) must be installed.

```bash
# Debian/Ubuntu
sudo apt update && sudo apt install ffmpeg

# Fedora
sudo dnf install ffmpeg

# Arch/Manjaro
sudo pacman -S ffmpeg

# openSUSE
sudo zypper install ffmpeg
```

## Installation

1. Save the script as `StartUpSoundShuffler`  
2. Make it executable:
```bash
chmod +x StartUpSoundShuffler
```
3. Place it in `~/.local/bin`:
```bash
mkdir -p ~/.local/bin
mv StartUpSoundShuffler ~/.local/bin/
```

### Why `~/.local/bin`?
- Standard location for personal scripts on Linux  
- Does not require administrative privileges  
- Usually included in your PATH by default  

If `~/.local/bin` is not in your PATH, add this to your shell config:
```bash
export PATH="$HOME/.local/bin:$PATH"
```

## Setting up your sounds

The `.sounds` folder in your home directory is **not** a standard Linux directory. You must create it and add sounds before the program can play anything.  

You can let the script create it automatically on first run or do it yourself:
```bash
mkdir -p ~/.sounds
cp /path/to/sound1.mp3 ~/.sounds/
cp /path/to/sound2.wav ~/.sounds/
```

## Testing the script

Run it in a terminal:
```bash
StartUpSoundShuffler
```
If the `.sounds` folder is empty or `ffplay` is missing, the script will explain what to fix.  

## Running at login

### Option 1: systemd user service
```bash
mkdir -p ~/.config/systemd/user
nano ~/.config/systemd/user/startupsoundshuffler.service
```
Paste:
```ini
[Unit]
Description=Random startup sound

[Service]
Type=oneshot
ExecStart=%h/.local/bin/StartUpSoundShuffler

[Install]
WantedBy=default.target
```
Enable it:
```bash
systemctl --user daemon-reload
systemctl --user enable startupsoundshuffler.service
```

### Option 2: cron @reboot
```bash
crontab -e
```
Add:
```cron
@reboot /bin/sh -lc "~/.local/bin/StartUpSoundShuffler"
```

## Feedback and issues

If you encounter a problem, have a suggestion, or want to request a feature, please open an issue in this repository. Feedback is welcome and helps improve StartUpSoundShuffler for everyone.  
