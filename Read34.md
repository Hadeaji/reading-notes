# API Deployment

## Managing Django Settings: Issues

**Different environments.** Usually, you have several environments: local, dev, ci, qa, staging, production, etc. Each environment can have its own specific settings

**Sensitive data.** You have SECRET_KEY in each Django project. On top of this there can be DB passwords and tokens for third-party APIs like Amazon or Twitter. This data cannot be stored in VCS.

**Sharing settings between team members.** You need a general approach to eliminate human error when working with the settings. For example, a developer may add a third-party app or some API integration and fail to add specific settings. On large (or even mid-size) projects, this can cause real issues.

**Django settings are a Python code.** This is a curse and a blessing at the same time. It gives you a lot of flexibility, but can also be a problem – instead of key-value pairs, settings.py can have a very tricky logic.

## Setting Configuration: Different Approaches

There is no built-in universal way to configure Django settings without hardcoding them. But books, open-source and work projects provide a lot of recommendations and approaches on how to do it best.

- **settings_local.py**

This is the oldest method. I used it when I was configuring a Django project on a production server for the first time. I saw a lot of people use it back in the day, and I still see it now.

The basic idea of this method is to extend all environment-specific settings in the settings_local.py file, which is ignored by VCS.

- Pros:
    Secrets not in VCS.

- Cons:
    settings_local.py is not in VCS, so you can lose some of your Django environment settings.
    The Django settings file is a Python code, so settings_local.py can have some non-obvious logic.
    You need to have settings_local.example (in VCS) to share the default configurations for developers.

- **Separate settings file for each environment**

This is an extension of the previous approach. It allows you to keep all configurations in VCS and to share default settings between developers.

In this case, you make a settings package with the following file structure:

```
settings/
   ├── __init__.py
   ├── base.py
   ├── ci.py
   ├── local.py
   ├── staging.py
   ├── production.py
   └── qa.py
```

settings/local.py:

```
from .base import *


ALLOWED_HOSTS = ['localhost']
DEBUG = True
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'local_db',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
```

- Pros:
    All environments are in VCS.
    It’s easy to share settings between developers.

- Cons:
    You need to find a way to handle secret passwords and tokens.
    “Inheritance” of settings can be hard to trace and maintain.

## 12 Factors

12 Factors is a collection of recommendations on how to build distributed web-apps that will be easy to deploy and scale in the Cloud. It was created by Heroku, a well-known Cloud hosting provider.

As the name suggests, the collection consists of twelve parts:

1. Codebase
2. Dependencies
3. Config
4. Backing services
5. Build, release, run
6. Processes
7. Port binding
8. Concurrency
9. Disposability
10. Dev/prod parity
11. Logs
12. Admin processes

## django-environ

Based on the above, we see that environment variables are the perfect place to store settings.

Now it’s time to talk about the toolkit.

Writing code using os.environ could be tricky sometimes and require additional effort to handle errors. It’s better to use django-environ instead.

Technically it’s a merge of:

- envparse
- honcho
- dj-database-url
- dj-search-url
- dj-config-url
- django-cache-url

## Setting Structure

Instead of splitting settings by environments, you can split them by the source: Django, third- party apps (Celery, DRF, etc.), and your custom settings.

## Django Settings: Best practices

- Keep settings in environment variables.
- Write default values for production configuration (excluding secret keys and tokens).
- Don’t hardcode sensitive settings, and don’t put them in VCS.
- Split settings into groups: Django, third-party, project.
- Follow naming conventions for custom (project) settings.

# What is SSH

SSH, or Secure Shell, is a remote administration protocol that allows users to control and modify their remote servers over the Internet. The service was created as a secure replacement for the unencrypted Telnet and uses cryptographic techniques to ensure that all communication to and from the remote server happens in an encrypted manner.

The Figure Below shows a typical SSH Window. Any Linux or macOS user can SSH into their remote server directly from the terminal window. Windows users can take advantage of SSH clients like Putty.

## How Does SSH Work

If you’re using Linux or Mac, then using SSH is very simple. If you use Windows, you will need to utilize an SSH client to open SSH connections. The most popular SSH client is PuTTY, which you can learn more about here.

For Mac and Linux users, head over to your terminal program and then follow the procedure below:

The SSH command consists of 3 distinct parts:

`ssh {user}@{host}`

The SSH key command instructs your system that you want to open an encrypted Secure Shell Connection. {user} represents the account you want to access.

## Understanding Different Encryption Techniques

The significant advantage offered by SSH over its predecessors is the use of encryption to ensure secure transfer of information between the host and the client. Host refers to the remote server you are trying to access, while the client is the computer you are using to access the host. There are three different encryption technologies used by SSH:

1. Symmetrical encryption
2. Asymmetrical encryption
3. Hashing.

### Symmetric Encryption

Symmetric encryption is a form of encryption where a secret key is used for both encryption and decryption of a message by both the client and the host. Effectively, any one possessing the key can decrypt the message being transferred.

Symmetric keys are used to encrypt the entire communication during a SSH Session. Both the client and the server derive the secret key using an agreed method, and the resultant key is never disclosed to any third party. The process of creating a symmetric key is carried out by a key exchange algorithm.

> What makes this algorithm particularly secure is the fact that the key is never transmitted between the client and the host.

### Asymmetric Encryption

Unlike symmetrical encryption, asymmetrical encryption uses two separate keys for encryption and decryption. These two keys are known as the public key and the private key. Together, both these keys form a public-private key pair.

The public key, as the name suggest is openly distributed and shared with all parties. While it is closely linked with the private key in terms of functionality, the private key cannot be mathematically computed from the public key. The relation between the two keys is highly complex: a message that is encrypted by a machine’s public key, can only be decrypted by the same machine’s private key.

Unlike the general perception, asymmetrical encryption is not used to encrypt the entire SSH session. Instead, it is only used during the key exchange algorithm of symmetric encryption. Before initiating a secured connection, both parties generate temporary public-private key pairs, and share their respective private keys to produce the shared secret key.

### Hashing

One-way hashing is another form of cryptography used in Secure Shell Connections. One-way-hash functions differ from the above two forms of encryption in the sense that they are never meant to be decrypted. They generate a unique value of a fixed length for each input that shows no clear trend which can exploited. This makes them practically impossible to reverse.

It is easy to generate a cryptographic hash from a given input, but impossible to generate the input from the hash. This means that if a client holds the correct input, they can generate the crypto-graphic hash and compare its value to verify whether they possess the correct input.

## How Does SSH Work with These Encryption Techniques

The way SSH works is by making use of a client-server model to allow for authentication of two remote systems and encryption of the data that passes between them.

SSH operates on TCP port 22 by default (though this can be changed if needed). The host (server) listens on port 22 (or any other SSH assigned port) for incoming connections. It organizes the secure connection by authenticating the client and opening the correct shell environment if the verification is successful.

### Session Encryption Negotiation

When a client tries to connect to the server via TCP, the server presents the encryption protocols and respective versions that it supports. If the client has a similar matching pair of protocol and version, an agreement is reached and the connection is started with the accepted protocol. The server also uses an asymmetric public key which the client can use to verify the authenticity of the host.

Once this is established, the two parties use what is known as a Diffie-Hellman Key Exchange Algorithm to create a symmetrical key.

This algorithm allows both the client and the server to arrive at a shared encryption key which will be used henceforth to encrypt the entire communication session.

Here is how the algorithm works at a very basic level:

1. Both the client and the server agree on a very large prime number, which of course does not have any factor in common. This prime number value is also known as the seed value.
2. Next, the two parties agree on a common encryption mechanism to generate another set of values by manipulating the seed values in a specific algorithmic manner. These mechanisms, also known as encryption generators, perform large operations on the seed.
3. Both the parties independently generate another prime number. This is used as a secret private key for the interaction.
4. This newly generated private key, with the shared number and encryption algorithm (e.g. AES), is used to compute a public key which is distributed to the other computer.
5. The parties then use their personal private key, the other machine’s shared public key and the original prime number to create a final shared key. This key is independently computed by both computers but will create the same encryption key on both sides.
6. Now that both sides have a shared key, they can symmetrically encrypt the entire SSH session. The same key can be used to encrypt and decrypt messages.
