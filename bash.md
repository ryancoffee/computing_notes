# .bashrc modifications  

```bash
parse_git_branch() {
	git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}

if [ "$color_prompt" = yes ]; then
	PS1="${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\W\[\033[00m\]\[\033[33m\]\$(parse_git_branch)\[\033[00m\]\$ "
else
	PS1='${debian_chroot:+($debian_chroot)}\u@\h:\W\$ '
fi
unset color_prompt force_color_prompt
```

## cool 
read command to get a string from the command line
```bash
read response 
echo $response
```
# Setting up home cluster
```
https://www.blasbenito.com/post/01_home_cluster/
```

# Executing bash commands remotely
```bash
ssh -t username@host 'top'
ssh -t roaster 'sudo apt update; sudo apt -y upgrade'
```

```
https://www.cnet.com/news/ssh-tip-send-commands-remotely/
https://www.linuxtechi.com/execute-linux-commands-remote-system-over-ssh/
https://www.cyberciti.biz/faq/unix-linux-execute-command-using-ssh/
https://www.techrepublic.com/article/how-to-run-a-command-that-requires-sudo-via-ssh/
```
