# Photocopier

Photocopier provides handy FTP/SSH adapters to abstract away file and directory copying.
To move directories to/from the remote server, it wraps efficient tools like lftp and rsync.

## Installation

Add this line to your application's Gemfile:

    gem 'photocopier'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install photocopier

## Usage

```ruby
require 'photocopier'

ssh = Photocopier::SSH.new(
  host: 'my_host',
  user: 'my_user'
)

# downloads a file and returns the content
puts ssh.get('remote_file.txt')

# downloads a file and saves the content
ssh.get('remote_file.txt', './local_file.txt')

# uploads a file with the specified content
ssh.put('foobar!', 'remote_file.txt')

# uploads a file
ssh.put('./local_file.txt', 'remote_file.txt')

# mirros the remote directory into the local machine (needs rsync on the local machine)
ssh.get_directory('remote_dir', './local_dir')

# and viceversa
ssh.put_directory('./local_dir', 'remote_dir')
```
The very same commands are valid for the `Photocopier::FTP` adapter.

## FTP

`Photocopier::FTP.new` accepts the following parameters (you need to pass them
all).

```ruby
{
  host: '',
  user: '',
  password: ''
}

## SSH Gotchas

`Photocopier::SSH.new` accepts the following parameters (you DON'T need
to pass them all).

```ruby
{
  host: '',
  user: '',
  password: '',
  port: '',
  gateway: {
    host: '',
    user: '',
    password: '',
    port: ''
  }
}
```

**TL;DR:** Avoid specifying passwords on Photocopier::SSH, and use more secure
and reliable ways to authenticate (`ssh-copy-id` anyone?).

The passwords specified will be used during `.get` and `.put` commands (which use
the pure-ruby `net-ssh` gem). For performance reasons, the `.get_directory` and
`.put_directory` commands make use of `rsync`. There's no easy way to pass SSH
passwords to rsync: the only way is to install a tool called [`sshpass`](http://sourceforge.net/projects/sshpass/)
on your machine (and on the gateway machine, if you also need to specify the password
of the final machine).

On Linux, you can install it with your standard package manager. On Mac, you can
have it via [`brew`](https://github.com/mxcl/homebrew):

```
sudo brew install https://raw.github.com/gist/1513663/3e98bf9e03feb7e31eeddcd08f89ca86163a376d/sshpass.rb
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
