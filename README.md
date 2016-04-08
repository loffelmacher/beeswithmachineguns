# Bees with Machine Guns!

Creates many Amazon EC2 instances in order to run a distributed set of HTTP requests. Originally [forked from here](https://github.com/newsapps/beeswithmachineguns).


## Dependencies

* Python 2.6 (2.7 works too)
* boto
* paramiko

## Installation for users

Preferred:

```
pip install beeswithmachineguns
```

or, if you must:

```
easy_install beeswithmachineguns
```

## Installation for developers (w/ virtualenv + virtualenvwrapper)

```
git clone git://github.com/newsapps/beeswithmachineguns.git
cd beeswithmachineguns
mkvirtualenv --no-site-packages bees
easy_install pip
pip install -r requirements.txt
```

## Configuring AWS credentials

Bees uses boto to communicate with EC2 and thus supports all the same methods of storing credentials that it does.  These include declaring environment variables, machine-global configuration files, and per-user configuration files. You can read more about these options on "boto's configuration page":http://code.google.com/p/boto/wiki/BotoConfig.

At minimum, create a .boto file in your home directory with the following contents:

```
[Credentials]
aws_access_key_id = <your access key>
aws_secret_access_key = <your secret key>
```

The credentials used must have sufficient access to EC2.

Make sure the .boto file is only accessible by the current account:

```
chmod 600 .boto
```

## Usage

A typical bees session looks something like this:

```
bees up -s 4 -g public -k frakkingtoasters
bees attack -n 10000 -c 250 -u http://www.ournewwebbyhotness.com/
bees down
```

This spins up 4 servers in security group 'public' using the EC2 keypair 'frakkingtoasters', whose private key is expected to reside at ~/.ssh/frakkingtoasters.pem.

*Note*: the default EC2 security group is called 'default' and by default it locks out SSH access. I recommend creating a 'public' security group for use with the bees and explicitly opening port 22 on that group.

It then uses those 4 servers to send 10,000 requests, 250 at a time, to attack OurNewWebbyHotness.com.

Lastly, it spins down the 4 servers.  *Please remember to do this*--we aren't responsible for your EC2 bills.

The (nearly) complete list of options includes:

```
-k --key | The ssh key pair name to use to connect to the new servers.
-s --servers | The number of servers to start (default: 5).
-g --group | The security group(s) to run the instances under (default: default).
-z --zone | The availability zone to start the instances in (default: us-east-1d).
-i --instance | The instance-id to use for each server from (default: ami-ff17fb96).
-t --type | The instance-type to use for each server (default: t1.micro).
-l --login | The ssh username name to use to connect to the new servers (default: newsapps).
-v --subnet | The vpc subnet id in which the instances should be launched. (default: None).
-u --url | URL of the target to attack.
-p --post-file | The POST file to deliver with the bee's payload.
-m --mime-type | The MIME type to send with the request.
-n --number | The number of total connections to make to the target (default: 1000).
-C --cookies | Cookies to send during http requests. The cookies should be passed using standard cookie formatting, separated by semi-colons and assigned with equals signs.
-c --concurrent | The number of concurrent connections to make to the target (default: 100).
-H --headers | HTTP headers to send to the target to attack. Multiple headers should be separated by semi-colons, e.g header1:value1;header2:value2
-e --CSV | Store the distribution of results in a csv file for all completed bees (default: '').
-T --tpr | The upper bounds for time per request. If this option is passed and the target is below the value a 1 will be returned with the report details (default: None)
-R --rps | The lower bounds for request per second. If this option is passed and the target is above the value a 1 will be returned with the report details (default: None).
```

## The caveat! (PLEASE READ)

(The following was cribbed from our "original blog post about the bees":http://blog.apps.chicagotribune.com/2010/07/08/bees-with-machine-guns/.)

If you decide to use the Bees, please keep in mind the following important caveat: they are, more-or-less a distributed denial-of-service attack in a fancy package and, therefore, if you point them at any server you don’t own you will behaving *unethically*, have your Amazon Web Services account *locked-out*, and be *liable* in a court of law for any downtime you cause.

You have been warned.

## Bugs

Please log your bugs on the "Github issues tracker":http://github.com/newsapps/beeswithmachineguns/issues.

## Credits

The bees are a creation of the News Applications team at the Chicago Tribune--visit "our blog":http://apps.chicagotribune.com/ and read "our original post about the project":http://blog.apps.chicagotribune.com/2010/07/%2008/bees-with-machine-guns/.

Initial refactoring code and inspiration from "Jeff Larson":http://github.com/thejefflarson.

Thanks to everyone who reported bugs against the alpha release.

## License

MIT.
