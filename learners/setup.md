---
title: Setup
---

## Installing Git

Since several Carpentries lessons rely on Git, please see
[this section of the workshop template][workshop-setup] for
instructions on installing Git for various operating systems.

- [Git installation on Windows][workshop-setup]
- [Git installation on MacOS][workshop-setup]
- [Git installation on Linux][workshop-setup]

Your institution may have already installed git on your work computer.
Ask your instructor if this step is necessary or simply type:

```bash
$ git --version
git version 2.47.0
```

If a version number is printed like the output above git is installed and 
ready to use.

## Creating a GitHub Account

You will need an account for [GitHub](https://github.com) to follow episodes 7 & 8 in this lesson.

1. Go to <https://github.com> and follow the "Sign up" link at the top-right of the window.
2. Follow the instructions to create an account.
3. Verify your email address with GitHub.
4. Configure multifactor authentication and or a passkey (see below).

There is no fixed guidance for choosing your GitHub username however you should ensure it is suitable for work.
At the Met Office a common pattern for usernames is: `mo-{first name initial}{surname}`.
So if your name is `Eleanor Ormerod` your username would be: `mo-eormerod`

### Multi-factor Authentication

In 2023, GitHub introduced a requirement for 
all accounts to have 
[multi-factor authentication (2FA)](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/about-two-factor-authentication) 
configured for extra security.
Several options exist for setting up 2FA, which are summarised here:

1. If you already use an authenticator app, 
   like [Google Authenticator](https://support.google.com/accounts/answer/1066447?hl=en&co=GENIE.Platform%3DiOS&oco=0) 
   or [Duo Mobile](https://duo.com/product/multi-factor-authentication-mfa/duo-mobile-app) on your smartphone for example, 
   [add GitHub to that app](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-totp-mobile-app).
2. If you have access to a smartphone but do not already use an authenticator app, install one and 
   [add GitHub to the app](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-totp-mobile-app).
3. If you do not have access to a smartphone or do not want to install an authenticator app, you have two options:
    1. [set up 2FA via text message](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-text-messages) 
       ([list of countries where authentication by SMS is supported](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/countries-where-sms-authentication-is-supported)), or
    2. [use a hardware security key](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-security-key) 
       like [YubiKey](https://www.yubico.com/products/yubikey-5-overview/) 
       or the [Google Titan key](https://store.google.com/us/product/titan_security_key?hl=en-US&pli=1).

The GitHub documentation provides [more details about configuring 2FA](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication).

### Passkeys

To completely avoid having authentication for work purposes on a personal device you may choose to [set up a passkey](https://docs.github.com/en/authentication/authenticating-with-a-passkey/managing-your-passkeys). Your instructor and organisation will be able to provide guidance on suitable passkey providers and password managers.

----------------

## Preparing Your Working Directory

We'll do our work in the `Desktop` folder so make sure you change your working directory to it with:

```bash
$ cd
$ cd Desktop
```

[workshop-setup]: https://carpentries.github.io/workshop-template/install_instructions/#git

