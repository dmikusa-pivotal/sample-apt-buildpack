# Apt Buildpack Sample

This is a small demo that works with the [apt-buildpack](https://github.com/cloudfoundry/apt-buildpack). This is a buildpack that allows you to install packages using `apt`, which can be useful when the stock `cflinuxfs3` image does not provide you with a required library or tool.

## Usage

Run `cf push -m 64m -b https://github.com/cloudfoundry/apt-buildpack#v0.2.2 -b binary_buildpack --random-route -c 'sleep 999999' -u none apt-bp-sample`.

To test, run `cf ssh apt-bp-sample -t -c "/tmp/lifecycle/launcher /home/vcap/app bash ''"`. This will get you a shell in the application container and it will source and properly set up PATH. You should now be able to run `mysql --help` or `/home/vcap/deps/0/apt/opt/mssql-tools/bin/sqlcmd`. Note how this requires a full path because the deb file doesn't install binaries into a standard location.

## Details

When run, cf cli will push up the `apt.yml` file which is read by the apt buildpack. The buildpack will then go through and install any packages listed in the `apt.yml` file. In this case, we're instructing it to install the `mysql-client` and MSSQL ODBC packages. The latter is an example of installing packages from an external apt repository.

The `binary_buildpack` is required because the apt buildpack is only a supply buildpack and doesn't actually know how to run anything. It's a technicality which we can bypass by adding the `binary_buildpack`.
