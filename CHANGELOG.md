# Changelog

## 0.9.x 

* 0.9.2 (19/Aug/2016) : No changes, just corrected date in gemspec.

* 0.9.1 (17/Aug/2016) : Added `deep_merge` flag to optionally merge hash values, instead of replacing them with values from a higher priority plugin ([issue #38](https://github.com/markround/tiller/issues/38)). Thanks to [pgleyzer](https://github.com/pgleyzer) for raising this issue.

* 0.9.0 (10/Aug/2016)
  * Breaking change : Value precedence has changed. Previously, global values were merged together in the order that plugins were loaded. Then, the same was done for template values. Finally, template values were merged over the top of global values. This led to some counter-intuitive behaviour, such as a template value being defined in a defaults section, but still taking priority over a global value supplied by a higher priority plugin (like the environment plugin). Now, the behaviour has been simplified : We go through the plugins in order, and for each one we merge template values over global values, then proceed onto the next plugin. In summary: A template value will take priority over a global value, and any value from a plugin loaded later will take priority over any previously loaded plugins. Many thanks again to [Eugen Mayer](https://github.com/EugenMayer) for his suggestion on cleaning up this behaviour.


## 0.8.x

* 0.8.0 (28/Jul/2016)
  * Breaking change : Switched to using a singleton pattern for logging and config variables. See http://www.markround.com/blog/2016/07/27/tiller-0-dot-8-changes-for-custom-plugins/ - you'll need to do a `s/\@log/Tiller::log/g` and `s/\@config/Tiller::config/g` on any custom plugins you may have written. If you've not written any custom plugins and are just using the ones bundled with Tiller, you can ignore this.
  * [Sub-templating](README.md#sub-templates) feature, as requested by [Pablo Gonzalez](https://github.com/kydorn) - thanks for the idea!
  * [Helper modules](docs/developers.md#helper-modules) feature, lets you group together blocks of Ruby code which you can call from your templates.



## 0.7.x

* 0.7.10 : Added optional MD5 sum checking and "no-exec if no templates changed" feature (https://github.com/markround/tiller/issues/30), "stub" environment support (https://github.com/markround/tiller/issues/29) and [environment plugin](docs/plugins/environment.md) configuration parameters `prefix` and `lowercase` to control behaviour of environment variables. Many thanks to [Eugen Mayer](https://github.com/EugenMayer) and Hermes42 for their suggestions.
* 0.7.9 : Added external_file plugin.
* 0.7.8 : Consul plugin! See [this blog post](http://www.markround.com/blog/2016/05/12/new-consul-plugin-for-tiller) for an introduction and walk-through.
* 0.7.7 : a nice feature suggested by [kydorn](https://github.com/kydorn) in [markround/tiller#18](https://github.com/markround/tiller/issues/18). Now the file datasource supports a global_values: block, so you can create defaults per environment (and use the defaults datasource to provide a default for all environments). Thanks for the suggestion!
* 0.7.6 : The environment_json plugin now supports splitting the JSON structure into global and per-template local values.
* 0.7.5 : Tiller now returns with the status code of the child command. Otherwise, Tiller always exited with status 0, even if the child process itself failed.
* 0.7.4 :  Not strictly a feature, but previous versions of Tiller didn't throw an error when we had an empty template config definition in a defaults file. Tiller 0.7.3 "broke" this, so while it's arguably the correct thing to bail out, in the interests of backwards-compatibility, we now instead log a warning and continue.
* 0.7.3 : Added target_values support to defaults datasource. Now makes it easy to install a template in all environments.
* 0.7.2 : Added xml_file datasource. Thanks to Anthony Burns at clearme.com for the suggestion!
* 0.7.1 : Small bug-fix; hash was not being cleared in a loop due to not being initialised properly.
* 0.7.0 : Lots of changes!
  * Added support to read all configuration from a single common.yaml file
  * Switched to Logger for debug/verbose messages
  * Internal code cleanup
  * More cucumber/aruba tests.
  
## 0.6.x

* 0.6.5 : Bug fix for [issues/14](https://github.com/markround/tiller/issues/14). Thanks to [gnomff](https://github.com/gnomff) for spotting and reporting this.
* 0.6.4 : Added -x/--exec flag to override the default exec parameter. Thanks Navin Desai for the suggestion!
* 0.6.3 : Bug-fix: 0.6.2 introduced a nasty typo that stopped defaults.d merging. Thanks to weiyinteo for reporting this!
* 0.6.2 : Bug-fix: Handle empty defaults YAML files.
* 0.6.1 : Added HTTP plugins to retrieve templates and values from a webserver.
* 0.6.0 : Internal changes and optimisations. Added ZooKeeper data and template sources.
  
## 0.5.x

* 0.5.2 : Author name change; I got married :)
* 0.5.1 : Added ability to specify exec: parameter as an array, which avoids spawning a subshell.
* 0.5.0 : Added common: override in each environment file, switched to using spawn, dropped support for Ruby < 1.9.2
  
## 0.4.x

* 0.4.0 : Changed default environment to "development" and added signal-catching behaviour.Tiller now catches the INT,TERM and HUP signals and passes them on to the child process spawned through exec. This helps avoid the "PID 1" problem by making sure that if Tiller is killed then the child process should also exit.

## 0.3.x

* 0.3.2 : Added default_environment (https://github.com/markround/tiller/issues/5)
* 0.3.1 : Internal code cleanup and refactor
* 0.3.0 : Added defaults datasource and modified class loading behaviour, so plugins are used in the order they are specified in common.yaml. See [http://www.markround.com/blog/2014/12/05/tiller-0.3.0-and-new-defaults-datasource](http://www.markround.com/blog/2014/12/05/tiller-0.3.0-and-new-defaults-datasource)
.

## 0.2.x

* 0.2.5 : Minor code cleanup, allow config hash to be null in environments
* 0.2.4 : Bug fix case of 'Oj' gem
* 0.2.3 : Catch Exceptions in API accept loop, dump JSON using Oj gem if it's installed, which resolves various `Encoding::UndefinedConversionError` problems.
* 0.2.2 : API Bind to all addresses, otherwise won't work in Docker container.
* 0.2.0 : Added HTTP API for querying status of Tiller from within a running container. Modified fork behaviour so that Tiller now spawns a child process and waits on it, so that the API can continue to run in a separate thread. Tidied up internal data structures (`common_config` now merged into main `config` hash).

## 0.1.x

* 0.1.5 : Added newline suppression option to ERb templates with the `-%>` closing tag, mimicking Rails behaviour.
* 0.1.4 : Added new command-line arguments -b, -l and -e which can be used to set tiller_base, tiller_lib and environment, instead of using the environment variables. Also added environment_json data source; see [http://www.markround.com/blog/2014/10/17/building-dynamic-docker-images-with-json-and-tiller-0-dot-1-4/](http://www.markround.com/blog/2014/10/17/building-dynamic-docker-images-with-json-and-tiller-0-dot-1-4/)
* 0.1.3 : Added "no exec" feature and command-line arguments -n, -v, and -h. Made output less verbose by default.
* 0.1.2 : Code cleanup based on input from RuboCop
* 0.1.1 : Very minor code cleanup
* 0.1.0 : Modified plugin API slightly by creating a `setup` hook which is called after the plugin is initialized. This could be useful for connecting to a database, parsing configuration files or setting up other data structures.

## 0.0.x

* 0.0.8 : Added `RandomDataSource` that wraps Ruby's `SecureRandom` class to provide UUIDs, random Base64 encoded data and so on.
* 0.0.7 : Added `EnvironmentDataSource`, so you can now use environment variables in your templates, e.g. `<%= env_user %>`. See [http://www.markround.com/blog/2014/08/04/tiller-v0-dot-0-7-now-supports-environment-variables/](http://www.markround.com/blog/2014/08/04/tiller-v0-dot-0-7-now-supports-environment-variables/) for a very quick overview.

