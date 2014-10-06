Ansible Playbook for Splunk
==============

This Ansible playbook installs and configures a Splunk 6.1 cluster


So far I've only used and tested this playbook on AWS instances. 

* The [core](https://github.com/alanwill/cfn-core) CloudFormation template is used to create the VPC
* The [cfn-splunk](https://github.com/alanwill/cfn-splunk) CloudFormation template is used to create the Splunk components

You don't have to use the above templates and can surely use a pre-created VPC and instances, just be sure that your instances are tagged as Ansible expects or tweak the splunk-site.yml file to adapt to your tagging convention.

This playbook will do the following:

* Install the latest OS security updates on all instances
* Update the hostsnames on all instances to be the EC2 instance ID
* Download and install the Splunk Enterprise RPM on all instances, if it's not already installed
* Delete the RPM after successful install
* Run config_splunk_inputs.sh which updates the inputs.conf on each component to include the instance hostname
* Start Splunk and set to auto-start on boot
* Update ACLs to allow Splunk to read /var/log files
* Reset the default Splunk password
* Copy custom configuration files (authentication.conf, web.conf, authorize.conf, ui-prefs.conf, alert_actions.conf)
* Copy custom certs for Splunk Web
* Add nodes to the License Master
* Restart Splunk
* Install packages to enable Cloudwatch metrics
* Configure the Cluster Master with a replication factor of 3 and search factor of 2 then restart Splunk on the instance
* Add the Search Heads to the cluster, then restart Splunk on them
* Add the Peer Nodes to the cluster
* Partition and mount the Peer node volumes
* Disable Splunk web on the Peer Nodes

By the time this playbook completes you'll have a working Splunk cluster.

##Future

There's a few things I'm looking to do to make this playbook more re-usable, namely:

* Increase the idempotency
* Make the peer node role more dynamic to various instance sizes. As it, it works best with i2.2xlarge instances
* Consolidate all variables to a single master file

##Contributing

I can't say this enough, Pull Requests are very much welcomed. Hope this playbook helps others as much as it helps me. If you have any feedback on ways to improve it, I'm all ears. Submit an Issue if something doesn't work as advertised.

alan





