
Friday, 15/05/2026, while manually update my Fedora system through dnf in the terminal, I was stopped by this error:

```
Transaction failed: Signature verification failed.
Public key "https://repo.librewolf.net/pubkey.gpg" is already present, not importing.
OpenPGP check for package "librewolf-150.0.3-1.x86_64" (/var/cache/libdnf5/librewolf-d3659b612308432a/packages/librewolf-150.0.3-1-linux-x86_64-rpm.rpm) from repo "librewolf" has failed: Public key is not installed.
```

I tried some generic fix, but failed. So as usual, I finding my way into librewolf issues tracker repo.

There 're 2 issues popping up after I search "Public key is not installed" :
- https://codeberg.org/librewolf/issues/issues/3034
- https://codeberg.org/librewolf/issues/issues/3026

According to the maintainer, the signing keys was exposed within one of their CI runs, the CI logs was removed but they do not sure if anyone has view the key

Source: https://chaos.social/@librewolf/116495829732331566
```
#LibreWolf v150.0.1-1 is now available!

https://
codeberg.org/librewolf/bsys6/releases/tag/150.0.1-1

https://
librewolf.net/installation/

No major changes from LibreWolf's end.

Unfortunately, we had to rotate one of our signing subkeys, as it was, for a very short period of time, exposed in a CI log. The CI log was swiftly removed with the help of Codeberg, but however unlikely it is, we cannot guarantee that another party hadn't viewed the key within that timeframe.

1/5
```


The fix is in [comment-14518221](https://codeberg.org/librewolf/issues/issues/3026#issuecomment-14518221) from user [blipblop](https://codeberg.org/blipblop)

```
Here's what I ended up doing:

    Find the offending key
    rpm -q gpg-pubkey --qf '%{NAME}-%{VERSION}-%{RELEASE}\t%{SUMMARY}\n' | grep -i libre

    Delete the offending key (sub with the key found in step 1)
    sudo rpm -e gpg-pubkey-XXXXXXXX-XXXXXXXX

    Re-import the new key
    sudo rpm --import https://repo.librewolf.net/pubkey.gpg
```
