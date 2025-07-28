# 🧠 Fixing `kubectl` Bash Autocompletion Issues

If you're experiencing broken autocompletion when you SSH into your Kubernetes master or worker nodes, this guide helps you **fix it once and for all**! 🛠️



## 🐞 Problem

Every time I SSH into my Kubernetes node and try to use `kubectl` with tab completion, I get this annoying error:

```bash
kubectl cre_get_comp_words_by_ref: command not found ❌
````

This means bash-completion isn't set up properly in your shell session.



## ✅ Solution (Permanent Setup)

Follow these steps to permanently enable `kubectl` autocompletion on every SSH login session:



### 1️⃣ Install `bash-completion` Package

Install the required package for enabling autocompletion.

**For Debian/Ubuntu:**

```bash
sudo apt-get update
sudo apt-get install bash-completion -y
```

**For Red Hat/CentOS:**

```bash
sudo yum install bash-completion -y
```

Then load it into your session:

```bash
source /etc/profile.d/bash_completion.sh
```

---

### 2️⃣ Enable Autocompletion in Shell

Edit your shell configuration file:

**If using `root`:**

```bash
vim /root/.bashrc
```

**If using a regular user:**

```bash
vim ~/.bashrc
```

At the **end of the file**, add the following block:

```bash
# ✅ Enable bash completion
if [ -f /etc/bash_completion ]; then
  . /etc/bash_completion
fi

# 🚀 Enable kubectl autocompletion
source <(kubectl completion bash)

# 🔁 Optional: alias for kubectl
alias k=kubectl
complete -F __start_kubectl k
```

> 💡 **Tip:** If you’re using `sudo su` or `sudo -i` to switch to root, make sure to put the same config in `/root/.bashrc`.

---

### 3️⃣ Reload the Configuration

To apply changes:

```bash
source ~/.bashrc
```

---

### 4️⃣ (Optional) Bash as Default Shell

Make sure your default shell is Bash:

```bash
echo $SHELL
```

If not, set it:

```bash
chsh -s /bin/bash
```



## 🧪 Test It!

Type the following and press `Tab` twice:

```bash
kubectl [TAB][TAB]
```

You should now see a list of available commands 🎉



## 📝 Summary

| Step | Action                                     |
| ---- | ------------------------------------------ |
| 1️⃣  | Install `bash-completion`                  |
| 2️⃣  | Add config to `.bashrc` or `.bash_profile` |
| 3️⃣  | Reload the shell config                    |
| 4️⃣  | Test autocompletion with `kubectl`         |



## 💬 Bonus

If you want to automate this across multiple nodes, consider writing a shell script to apply this config in `/root/.bashrc` for each host.


> ## 📝 About the Author
> #### Crafted with care and ❤️ by [Ali bahrami](https://github.com/alibahrami2). 👨‍💻
> If this repo saved you time or solved a problem, a ⭐ means everything in the DevOps world. 🧠💾
> Your star ⭐ is like a high five from the terminal — thanks for the support! 🙌🐧



