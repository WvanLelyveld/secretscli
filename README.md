[![Gem Version](https://badge.fury.io/rb/secrets_cli.svg)](https://badge.fury.io/rb/secrets_cli)

# SecretsCli

This is a CLI for easier use of [vault](https://www.vaultproject.io/)

There is also a mina plugin [mina-secrets](https://github.com/infinum/mina-secrets)

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'secrets_cli'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install secrets_cli

## Prerequisites

The following environment variables need to be set:

For `vault` itself:

    VAULT_ADDR - this is an address to your vault server

For `secrets_cli`:

    VAULT_AUTH_METHOD - this is auth method ('github', 'token' or 'app_id' supported for now)
    VAULT_AUTH_TOKEN - this is vault auth token
    VAULT_AUTH_APP_ID - machine app_id
    VAULT_AUTH_USER_ID - machine user_id which matches app_id

For github token you only need `read:org` permissions.

## Usage

All commands have `--help` with detailed descriptions of options.
Some of the commands have `--verbose` switch which will print out the commands it run.

### Init

    $ secrets init

This will create `.secrets` file with project configuration. The command will ask you all it needs to know if you do not
supply the config through options.

Example of the `.secrets`:

    ---
    :secrets_file: config/application.yml   # file where your secrets are kept, depending on your environment gem (figaro, dotenv, etc)
    :secrets_storage_key: rails/my_project/ # vault 'storage_key' where your secrets will be kept.

### Policies

    $ secrets policies

To get all the policies your auth grants please use this command.

### storage_keys and environments

Next 3 commands read and write to your project storage_key in vault. The value of the storage_key is generated by
secrets_storage_key + environment. Example:

      `rails/my_project/development`

Environment is `development` by default, but it can be overwriten by passing `--environment` option, or setting `RAILS_ENV` environment variable.

### Read

    $ secrets read

This will read development secrets from the vault.

To read secrets from a different environment, use the `-e` flag:

    $ secrets read -e production

### Pull

    $ secrets pull

This will pull from vault and write to your secrets file. The deafult file it will pull is the development one.

To pull from a different environment, also supply the `-e` flag and the `-f` flag for the file path:

    $ secrets pull -e production -f config/application.production.yml

### Push

    $ secrets push

This will push from your secrets file to vault.

The same flags apply for pushing as for pulling:

    $ secrets push -e production -f config/application.production.yml

## Development

After checking out the storage_key, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/infinum/secrets_cli. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
