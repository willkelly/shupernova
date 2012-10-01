## supernova - use novaclient with multiple nova environments the easy way

You may like *supernova* if you regularly have the following problems:

* You hate trying to source multiple novarc files when using *nova*
* You get your terminals confused and do the wrong things in the wrong nova environment
* You don't like remembering things
* You need to share common skeleton environment variables for *nova* with your teams
* You don't want to leave passwords lying around in plaintext.

If any of these complaints ring true, *supernova* is for you. *supernova* manages multiple nova environments without sourcing novarc's or mucking with environment variables.

### Installation

    $ cat bashrc_sample >> ~/.bashrc

### Configuration

Enabling encryption: Uncomment the gpg related lines in bashrc_sample.

For *supernova* to work properly, each environment must be defined in `~/.supernova` (in your user's home directory).  The data in the file is exactly the same as the environment variables which you would normally use when running *nova*.  You can copy/paste from your novarc files directly into configuration sections within `~/.supernova`.

Here's an example of two environments, **production** and **development**:

~/.supernova/production:

    -->cut<--
    NOVA_URL=http://production.nova.example.com:8774/v1.1/
    NOVA_VERSION=1.1
    NOVA_USERNAME = jsmith
    NOVA_API_KEY = fd62afe2-4686-469f-9849-ceaa792c55a6
    NOVA_PROJECT_ID = nova-production
    -->cut<--

~/.supernova/development

    -->cut<--
    NOVA_URL=http://dev.nova.example.com:8774/v1.1/
    NOVA_VERSION=1.1
    NOVA_USERNAME = jsmith
    NOVA_API_KEY = 40318069-6069-4d9f-836d-a46df17fc8d1
    NOVA_PROJECT_ID = nova-production
    -->cut<--

If you already have an existing novarc, you can use the "cp"
technology to turn it into a *supernova* profile.  These profiles will
be automatically encrypted as encountered if you enable the gpg options.

When you use *supernova*, you'll refer to these environments as **production** and **development**.  Every environment is specified by its configuration header name.

### Usage

    supernova [environment] [novaclient arguments...]

##### Passing commands to *nova*

For example, if you wanted to list all instances within the **production** environment:

    supernova production list

Show a particular instance's data in the preprod environment:

    supernova preprod show 3edb6dac-5a75-486a-be1b-3b15fd5b4ab0a

The first argument is generally the environment argument and it is expected to be a single word without spaces. Any text after the environment argument is passed directly to *nova*.

##### Listing your configured environments

You can list all of your configured environments by using the `ls` command.

#### A brief note about environment variables

*supernova* will only replace and/or append environment variables to the already present variables for the duration of the *nova* execution. If you have `NOVA_USERNAME` set outside the script, it won't be used in the script since the script will pull data from `~/.supernova` and use it to run *nova*. In addition, any variables which are set prior to running *supernova* will be left unaltered when the script exits.
