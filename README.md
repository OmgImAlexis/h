# h
Hosted apps done simply.

## Installation

I would suggest setting up your `origin` remote first so if you're using Github make sure to `git clone` before doing this.

```bash
# On the client connect to the server
ssh root@server_name.tld

# On the server run the following
mkdir /git
mkdir /workspace

# Run this to give yourself a helper function
# We'll use this below
makeRepo() {
    local name=$1
    local old_pwd=$PWD

    mkdir /git/$name.git
    cd /git/$name.git
    git init --bare
    curl https://raw.githubusercontent.com/OmgImAlexis/h/master/pre-receive?token=ABRZHZT2UTBTA6WPUU54CDS6N3L5U --output ./hooks/pre-receive
    sed -i "s/NAME=\"demo\"/NAME=\"$name\"/" ./hooks/pre-receive
    chmod +x ./hooks/pre-receive
    cd $old_pwd
}

# You'll need todo this for each of the projects you want to host on this machine
makeRepo "demo"
makeRepo "project1"

# Back on the client
cd ~/code/demo # Replace with your own
git remote add production root@server_name.tld:/git/demo
```

You should now be able to run `git push production` which will deploy your app to your server.