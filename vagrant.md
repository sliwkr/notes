# Vagrant snippet cookbook

## table of contents

- [inline shell provisioner example](#inline-shell-provisioner)

## snippets

### inline shell provisioner

```ruby
$alpine_python = <<SCRIPT
apk add python3
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.synced_folder ".", "/vagrant"
  config.vm.network "private_network", type: "dhcp"

  config.vm.define :alpine do |alpine|
    alpine.vm.box = "generic/alpine314"
    alpine.vm.provision "shell", inline: $alpine_python
  end
end
```
