# Local GitHub Account Configs

This is a Guide on how to configure multiple GitHub Accounts locally.

### **SSH Config**

#### **Step 1:** **Navigate to the .ssh Directory**

Open terminal and navigate to **.ssh**:

```
cd ~/.ssh
```

**Step 2: Create SSH Keys**

Generate SSH keys for each GitHub account:

```
ssh-keygen -t rsa -C "<your-email>" -f "github-<username1>"
ssh-keygen -t rsa -C "<your-email>" -f "github-<username2>"
```

* **-t rsa**: RSA encryption.
* **-C**: Email comment for identification.
* **-f:** Filename for keys.

#### **Step 3: Add SSH Keys to the Agent**

Start SSH agent:

```
eval "$(ssh-agent -s)"
```

Add keys to agent:

```
ssh-add --apple-use-keychain ~/.ssh/github-<username1>
ssh-add --apple-use-keychain ~/.ssh/github-<username2>
```

#### **Step 4: Add SSH Public Keys to GitHub**

Copy public keys to clipboard:

```
pbcopy < ~/.ssh/github-<username1>.pub
pbcopy < ~/.ssh/github-<username2>.pub
```

Add these to your different GitHub account’s SSH keys.

#### **Step 5: Configure SSH Alias**

Edit \~/.ssh/config:

```
vim ~/.ssh/config
```

Add configurations:

```
Host github.com-<username1>
    HostName github.com
    User git
    AddKeysToAgent yes
    IdentityFile ~/.ssh/github-<username1>
    IdentitiesOnly yes
```

```
Host github.com-<username2>
    HostName github.com
    User git
    AddKeysToAgent yes
    IdentityFile ~/.ssh/github-<username2>
    IdentitiesOnly yes
```

_IdentitiesOnly_ _yes_ is necessary to ensure only the specified key is used for authentication, preventing conflicts or errors when multiple keys are present.

#### **Step 6: Clone and Configure new Repositories**

Clone repository:

```
git clone git@github.com-<username1>:github-<username1>/{repo-name}.git
```

Check configuration:

```
git config --list
```

Set user details:

```
git config user.email "<your-email>"
git config user.name "<your-name>"
```

Repeat for all other Accounts and Repositories.

#### **Step 7: Change Remote URL for Existing Repositories**

```
git remote -v
git remote remove origin
git remote add origin git@github.com-<username1>:<username1>/{repo-name}.git
```

### **Git Config**

With this setup, your GitHub config gets picked up automatically for each repository, so you won’t have to configure user details every time.

Edit global .gitconfig:

```
nano ~/.gitconfig
```

Add includes:

```
[includeIf "hasconfig:remote.*.url:git@github-<username1>:*"]
  path = ~/.gitconfig-<username1>
```

```
[includeIf "hasconfig:remote.*.url:git@github-<username2>:*"]
  path = ~/.gitconfig-<username2>
```

Create account-specific configs:

* \~/.gitconfig-\<username1>:

```
[user]
  name = <your-name>
  email = <your-email>
```

* \~/.gitconfig-\<username2>:

```
[user]
  name = <your-name>
  email = <your-email>
```

Replace all placeholders _\<username1>_. Example:

```
git remote add origin git@github.com-devlifelore:devlifelore/knowledge-hub.git
```

Now you’re set to work with multiple GitHub accounts. Happy coding!

\
