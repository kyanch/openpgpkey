# Hosting a Web Key Directory on Github Pages


## Build WKD structure
```bash
cd <your repo>
mkdir -p .well-known/openpgpkey
cd .well-known

gpg --list-options show-only-fpr-mbox -k "@yourdomain.com" | $(gpgconf --list-dirs libexecdir)/gpg-wks-client -v --install-key
```

And then, you will get a directory like this:
```
.well-known/
└── openpgpkey
    └── kyanch.icu
        ├── hu
        │   └── s8y7oh5xrdpu9psba3i5ntk64ohouhga
        └── policy

4 directories, 2 files
```

## Build Github Pages

1. create `CNAME` point to openpgpkey.yourdomain.com
```bash
cd <youre repo>
echo "openpgpkey.yourdomain.com" > CNAME
```

2. prohibit jekyll build
```bash
touch .nojekyll
```

3. add a index.html (optional)
```bash
echo "<html>Hello WKD!</html>" > index.html
# or whatever html your want
# it only works for the page test.
```

4. Enable **Pages** with **https** in github repo settings

## Test your Build

1. Check Is Main Page works (if add index.html)
2. Obviously, you can fetch with curl
```bash
curl -L https://openpgpkey.yourdomain.com/.well-known/openpgpkey/yourdomain.com/policy
```
3. use `gpg` to add your key
```bash
gpg --auto-key-locate clear,wkd,nodefault --verbose --locate-key you@yourdomain.com
```
