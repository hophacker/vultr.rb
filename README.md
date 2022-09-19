# Vultr API Rubygem

[![Build Status](https://github.com/tolbkni/vultr.rb/workflows/Tests/badge.svg)](https://github.com/tolbkni/vultr.rb/actions)

The easiest and most complete rubygem for [Vultr](https://www.vultr.com). Currently supports [API v2](https://www.vultr.com/api/v2).

## Installation

Add this line to your application's Gemfile:

    gem 'vultr', github: "excid3/vultr.rb"

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install vultr

## Usage

To access the API, you'll need to create a `Vultr::Client` and pass in your API key. You can find your API key at [https://my.vultr.com/settings](https://my.vultr.com/settings/#settingsapi)

```ruby
$vultr = Vultr::Client.new(api_key: ENV["VULTR_API_KEY"])
```

The client then gives you access to each of the resources.

## Resources

The gem maps as closely as we can to the Vultr API so you can easily convert API examples to gem code.

Responses are created as objects like `Vultr::Account`. Having types like `Vultr::User` is handy for understanding what type of object you're working with. They're built using OpenStruct so you can easily access data in a Ruby-ish way.

##### Pagination

 `list` endpoints return pages of results. The result object will have a `data` key to access the results, as well as metadata like `next_cursor` and `prev_cursor` for retrieving the next and previous pages. You may also specify the

```ruby
results = $vultr.applications.list(per_page: 5)
#=> Vultr::Collection

results.total
#=> 48

results.data
#=> [#<Vultr::Application>, #<Vultr::Application>]

results.next_cursor
#=> "bmV4dF9fMTU="

# Retrieve the next page
$vultr.applications.list(per_page: 5, cursor: results.next_cursor)
#=> Vultr::Collection
```

### Account

```ruby
$vultr.account.info
```

### Applications

```ruby
$vultr.applications.list
```

### Backups

```ruby
$vultr.backups.list
$vultr.backups.get(backup_id: "id")
```

### Bare Metal

```ruby
$vultr.bare_metal.list
$vultr.bare_metal.create({})
$vultr.bare_metal.retrieve(baremetal_id: "id")
$vultr.bare_metal.update(baremetal_id: "id", {})
$vultr.bare_metal.delete(baremetal_id: "id")
$vultr.bare_metal.start(baremetal_id: "id")
$vultr.bare_metal.reboot(baremetal_id: "id")
$vultr.bare_metal.reinstall(baremetal_id: "id")
$vultr.bare_metal.halt(baremetal_id: "id")
$vultr.bare_metal.bandwidth(baremetal_id: "id")
$vultr.bare_metal.user_data(baremetal_id: "id")
$vultr.bare_metal.upgrades(baremetal_id: "id")
$vultr.bare_metal.vnc(baremetal_id: "id")
$vultr.bare_metal.list_ipv4(baremetal_id: "id")
$vultr.bare_metal.list_ipv6(baremetal_id: "id")

# Bulk operations
$vultr.bare_metal.start_instances(baremetal_ids: [id1, id2])
$vultr.bare_metal.halt_instances(baremetal_ids: [id1, id2])
$vultr.bare_metal.reboot_instances(baremetal_ids: [id1, id2])
```

### Block Storage

```ruby
$vultr.block_storage.list
$vultr.block_storage.create({})
$vultr.block_storage.retrieve(block_id: "id")
$vultr.block_storage.update(block_id: "id", {})
$vultr.block_storage.delete(block_id: "id")
$vultr.block_storage.attach(block_id: "id", {})
$vultr.block_storage.detach(block_id: "id")
```

### DNS (Domains)

```ruby
$vultr.dns.list
$vultr.dns.create({})
$vultr.dns.retrieve(dns_domain: "id")
$vultr.dns.update(dns_domain: "id", {})
$vultr.dns.delete(dns_domain: "id")
$vultr.dns.soa(dns_domain: "id")
$vultr.dns.update_soa(dns_domain: "id", {})
$vultr.dns.dnssec(dns_domain: "id")
$vultr.dns.list_records(dns_domain: "id")
$vultr.dns.retrieve_record(dns_domain: "id", record_id: "id")
$vultr.dns.create_record(dns_domain: "id", {})
$vultr.dns.update_record(dns_domain: "id", record_id: "id", {})
$vultr.dns.delete_record(dns_domain: "id", record_id: "id")
```

### Firewall

```ruby
$vultr.firewall.list
$vultr.firewall.create({})
$vultr.firewall.retrieve(firewall_group_id: "id")
$vultr.firewall.update(firewall_group_id: "id", {})
$vultr.firewall.delete(firewall_group_id: "id")
$vultr.firewall.list_rules(firewall_group_id: "id")
$vultr.firewall.create_rule(firewall_group_id: "id", {})
$vultr.firewall.retrieve_rule(firewall_group_id: "id", firewall_rule_id: "id")
$vultr.firewall.delete_rule(firewall_group_id: "id", firewall_rule_id: "id")
```

### Instances

```ruby
$vultr.instances.list
$vultr.instances.create({})
$vultr.instances.retrieve(instance_id: "id")
$vultr.instances.update(instance_id: "id", {})
$vultr.instances.delete(instance_id: "id")
$vultr.instances.start(instance_id: "id")
$vultr.instances.reboot(instance_id: "id")
$vultr.instances.reinstall(instance_id: "id")
$vultr.instances.restore(instance_id: "id", {})
$vultr.instances.halt(instance_id: "id")
$vultr.instances.bandwidth(instance_id: "id")
$vultr.instances.neighbors(instance_id: "id")
$vultr.instances.user_data(instance_id: "id")
$vultr.instances.upgrades(instance_id: "id")
$vultr.instances.list_ipv4(instance_id: "id")
$vultr.instances.create_ipv4(instance_id: "id", {})
$vultr.instances.delete_ipv4(instance_id: "id", ipv4: "id")
$vultr.instances.create_ipv4_reverse(instance_id: "id", {})
$vultr.instances.set_default_reverse_dns_entry(instance_id: "id", {})
$vultr.instances.list_ipv6(instance_id: "id")
$vultr.instances.ipv6_reverse(instance_id: "id")
$vultr.instances.create_ipv6_reverse(instance_id: "id", {})
$vultr.instances.delete_ipv6_reverse(instance_id: "id", ipv6: "id")
$vultr.instances.list_private_networks(instance_id: "id")
$vultr.instances.attach_private_network(instance_id: "id", network_id: "id")
$vultr.instances.detach_private_network(instance_id: "id", network_id: "id")
$vultr.instances.iso(instance_id: "id")
$vultr.instances.attach_iso(instance_id: "id", iso_id: "id")
$vultr.instances.detach_iso(instance_id: "id", iso_id: "id")
$vultr.instances.backup_schedule(instance_id: "id")
$vultr.instances.set_backup_schedule(instance_id: "id", {})

# Bulk operations
$vultr.instances.start_instances(instance_ids: [id1, id2])
$vultr.instances.halt_instances(instance_ids: [id1, id2])
$vultr.instances.reboot_instances(instance_ids: [id1, id2])
```

### ISO

```ruby
$vultr.iso.list
$vultr.iso.create({})
$vultr.iso.retrieve(iso_id: "id")
$vultr.iso.delete(iso_id: "id")
$vultr.iso.list_public
```

### Kubernetes

```ruby
$vultr.kubernetes.list
$vultr.kubernetes.create({})
$vultr.kubernetes.retrieve(vke_id: "id")
$vultr.kubernetes.update(vke_id: "id", {})
$vultr.kubernetes.delete(vke_id: "id")
$vultr.kubernetes.config(vke_id: "id")
$vultr.kubernetes.list_resources(vke_id: "id")
$vultr.kubernetes.list_node_pools(vke_id: "id")
$vultr.kubernetes.retrieve_node_pool(vke_id: "id", nodepool_id: "id")
$vultr.kubernetes.create_node_pool(vke_id: "id", {})
$vultr.kubernetes.update_node_pool(vke_id: "id", nodepool_id: "id", {})
$vultr.kubernetes.delete_node_pool(vke_id: "id", nodepool_id: "id")
$vultr.kubernetes.delete_node_pool_instance(vke_id: "id", nodepool_id: "id", node_id: "id")
$vultr.kubernetes.recycle_node_pool_instance(vke_id: "id", nodepool_id: "id", node_id: "id")
```

### Load Balancers

```ruby
$vultr.load_balancers.list
$vultr.load_balancers.create({})
$vultr.load_balancers.retrieve(load_balancer_id: "id")
$vultr.load_balancers.update(load_balancer_id: "id", {})
$vultr.load_balancers.delete(load_balancer_id: "id")
$vultr.load_balancers.list_forwarding_rules(load_balancer_id: "id")
$vultr.load_balancers.create_forwarding_rule(load_balancer_id: "id", {})
$vultr.load_balancers.retrieve_forwarding_rule(load_balancer_id: "id", forwarding_rule_id: "id")
$vultr.load_balancers.delete_forwarding_rule(load_balancer_id: "id", forwarding_rule_id: "id")
$vultr.load_balancers.list_firewall_rules(load_balancer_id: "id")
$vultr.load_balancers.retrieve_firewall_rule(load_balancer_id: "id", firewall_rule_id: "id")
```

### Object Storage

```ruby
$vultr.object_storage.list
$vultr.object_storage.create({})
$vultr.object_storage.retrieve(object_storage_id: "id")
$vultr.object_storage.update(object_storage_id: "id", {})
$vultr.object_storage.delete(object_storage_id: "id")
$vultr.object_storage.regenerate_keys(object_storage_id: "id")
$vultr.object_storage.list_clusters()
```

### Operating Systems

```ruby
$vultr.operating_systems.list
```

### Plans

```ruby
$vultr.plans.list
$vultr.plans.list_metal
```

### Private Networks

```ruby
$vultr.private_networks.list
$vultr.private_networks.create({})
$vultr.private_networks.retrieve(network_id: "id")
$vultr.private_networks.update(network_id: "id", {})
$vultr.private_networks.delete(network_id: "id")
```

### Regions

```ruby
$vultr.regions.list
$vultr.regions.list_availability(region_id: "id")
```

### Reserved IPs

```ruby
$vultr.reserved_ips.list
$vultr.reserved_ips.create({})
$vultr.reserved_ips.retrieve(reserved_ip: "id")
$vultr.reserved_ips.delete(reserved_ip: "id")
$vultr.reserved_ips.attach(reserved_ip: "id", instance_id: "id")
$vultr.reserved_ips.detach(reserved_ip: "id")
$vultr.reserved_ips.convert({})
```

### Snapshots

```ruby
$vultr.snapshots.list
$vultr.snapshots.create({})
$vultr.snapshots.retrieve(snapshot_id: "id")
$vultr.snapshots.create_from_url("https://...")
$vultr.snapshots.update(snapshot_id: "id", {})
$vultr.snapshots.delete(snapshot_id: "id")
```

### SSH Keys

```ruby
$vultr.ssh_keys.list
$vultr.ssh_keys.create({})
$vultr.ssh_keys.retrieve(ssh_key_id: "id")
$vultr.ssh_keys.update(ssh_key_id: "id", {})
$vultr.ssh_keys.delete(ssh_key_id: "id")
```

### Startup Scripts

```ruby
$vultr.startup_scripts.list
$vultr.startup_scripts.create({})
$vultr.startup_scripts.retrieve(startup_script_id: "id")
$vultr.startup_scripts.update(startup_script_id: "id", {})
$vultr.startup_scripts.delete(startup_script_id: "id")
```

### Users

```ruby
$vultr.users.list
$vultr.users.create({})
$vultr.users.retrieve(user_id: "id")
$vultr.users.update(user_id: "id", {})
$vultr.users.delete(user_id: "id")
```

## Contributing

1. Fork it ( https://github.com/tolbkni/vultr/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

When adding methods, add to the list of DEFINITIONS in lib/vultr.rb. Additionally, write a spec and add it to the list in the README.

## Thanks

Thanks Scott Motte and his [DigitalOcean](https://github.com/scottmotte/digitalocean) rubygem project.
