# passenger Attributes<a name="attributes-json-passenger"></a>

Contains a set of attributes that specify the Phusion Passenger configuration\.

**gem\_bin**  <a name="attributes-json-passenger-gem"></a>
The location of the RubyGems binaries, such as `"/usr/local/bin/gem"` \(string\)\.  

```
node["passenger"]["gem_bin"]
```

**max\_pool\_size**  <a name="attributes-json-passenger-max-pool"></a>
The maximum pool size \(number\)\.  

```
node["passenger"]["max_pool_size"]
```

**ruby\_bin**  <a name="attributes-json-passenger-ruby"></a>
The location of the Ruby binaries, such as `"/usr/local/bin/ruby"`\.  

```
node["passenger"]["ruby_bin"]
```

**version**  <a name="attributes-json-passenger-version"></a>
The Passenger version \(string\)\.  

```
node["passenger"]["version"]
```