# Dynamic Functions


A list of all dynamic functions supported by Cribl Search.

---

Dynamic scalar functions allow you to manipulate objects by operating on [`dynamic` values](dynamic), including dynamic arrays
and property bags.


| Name                                   | Description                                                                     |
| -------------------------------------- | ------------------------------------------------------------------------------- |
| [`bag_has_key`](bag-has-key)           | Checks whether a property bag contains a given key.                             |
| [`bag_keys`](bag-keys)                 | Lists all root keys of a property bag.                                          |
| [`bag_merge`](bag-merge)               | Merges multiple property bags, discarding duplicate keys.                       |
| [`bag_pack`](bag-pack)                 | Creates a property bag from an alternating list of keys and values.             |
| [`bag_pack_columns`](bag-pack-columns) | Creates a property bag from a list of columns.                                  |
| [`bag_remove_keys`](bag-remove-keys)   | Removes key-value pairs from a property bag.                                    |
| [`bag_set_key`](bag-set-key)           | Adds or overwrites a key-value pair in a property bag.                          |
| [`bag_zip`](bag-zip)                   | Creates a property bag from two dynamic arrays.                                 |
| [`make_bag`](make-bag)                 | Creates a property bag from multiple input bags.                                |
| [`make_bag_if`](make-bag-if)           | Creates a property bag from those input bags that meet the specified condition. |
| [`zip`](zip)                           | Merges dynamic arrays, grouping elements by index.                              |

The following [string functions](string-functions) support `dynamic` types as well:

| Function                                         | Usage with dynamic data types              |
| ------------------------------------------------ | ------------------------------------------ |
| [`` extractjson(`path,object`) ``](extract-json) | Uses path to navigate into object.         |
| [`` parse_json(`source`) ``](parse-json)         | Turns a JSON string into a dynamic object. |
| [`` range(`from,to,step`) ``](range)             | Generates an array of values.              |

