# Guide to Setting Up the `vsm` Function in Zsh

## Why I Made It

As someone who loves working with keyboard shortcuts and avoids using the mouse whenever possible, I found myself often needing to copy the output of terminal commands. The traditional way of doing this involves using the mouse to highlight the output and then copying it, which can be a bit tedious. So, I decided to create a shortcut that allows me to both display a command's output in the terminal and copy it to the clipboard using only the keyboard. If you're like me and prefer using keyboard shortcuts, you might find this useful as well.

## How to Set It Up

Here are the steps to set up the `vsm` function in Zsh:

1. **Install xclip**: The `xclip` command is used to interface with the clipboard. You can install it using the following command:
```bash
sudo apt-get install xclip
```
2. **Open your Zsh configuration file**: You can use a text editor like `nano` to open the `~/.zshrc` file:
```bash
nano ~/.zshrc
```
3. **Add the aliases and function**: Add the following lines at the end of the `~/.zshrc` file:
```bash
#Clip alias
alias xclip='xclip -selection clipboard'
alias clip='tee >(xclip)'

#function to enable vsm <command> to output command output and copy output to clipboard
function vsm()
{
    if [[ $1 == "curl" ]]; then
        command curl -s "${@:2}" | clip
    else
        "$@" | clip
    fi
}
```
4. **Save and close the file**: If you're using `nano`, you can do this by pressing `Ctrl+X`, then `Y`, then `Enter`.
5. **Make the changes take effect**: Run the following command to make the changes take effect in your current shell session:
```bash
source ~/.zshrc
```

Now, you can use the `vsm` function with any command to display that command's output in your terminal and copy it to your clipboard. For example, `vsm echo Hello, World!` would echo "Hello, World!" in your terminal and copy it to your clipboard.

## Adding Function and Alias System-Wide

If you want to make these changes available system-wide, you would need to add the aliases and function to the `/etc/zsh/zshrc` file instead (requires root permissions). Here's how you can do it in one command:

```bash
echo -e "\n#Clip alias\nalias xclip='xclip -selection clipboard'\nalias clip='tee >(xclip)'\n\n#function to enable vsm <command> to output command output and copy output to clipboard\nfunction vsm()\n{\n    if [[ \$1 == \"curl\" ]]; then\n        command curl -s \"\${@:2}\" | clip\n    else\n        \"\$@\" | clip\n    fi\n}" | sudo tee -a /etc/zsh/zshrc
```

This command will append the function and alias to the end of the `/etc/zsh/zshrc` file. Remember that modifying the `/etc/zsh/zshrc` file will affect all users on your system who use Zsh.

Also, remember that you'll need to run `source /etc/zsh/zshrc` or start a new shell session to make these system-wide changes take effect.

## Appending Text To A File In One Command

To append text to a file in one command, you can use this format:

```bash
echo -e "your text here" | sudo tee -a /path/to/your/file
```

In this command, `echo -e "your text here"` generates the text you want to append, and `sudo tee -a /path/to/your/file` appends that text to the end of your specified file.

Enjoy your new shortcuts! 😊
