> [Quoting krakjoe](https://www.reddit.com/r/PHP/comments/ampo3r/are_there_any_resources_on_the_zend_engine/efnqgsz)

- zend_vm_def.h contains the template used to generate the vm (zend_vm_execute.h), it's this file you change to introduce new instructions (php zend_vm_gen.php to generate zend_vm_execute.h after changes)
- zend_ast.{c,h} is the AST API
- zend_compile.{c,h} is the Compiler API
- zend_execute_API.{c,h} is the public executor API
- zend_closures.{c,h} Closure API, mostly closed/not exported though
- zend_types.h are typedef and constant type api bits
- zend_hash.{c,h} HashTable API
- zend_generators.{c,h} Generators API
- zend_alloc.{c,h} Zend MM API
- zend_extensions.{c,h} Zend Extensions API
- zend_objects_API.{c,h} Zend Objects API
- ext/opcache/Optimizer is the source for optimizer, and level above for opcache
