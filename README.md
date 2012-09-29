PuppetConf 2012 Notes
=====================

Day 1, Thursday Sept 27th
=========================

Keynote #1:  Luke Kanies, founder+ceo PuppetLabs
--------------------------------------------
@puppetmasetd
lots of community and outreach updates, puppetcamps (regional puppetconf's)
releases: PE 2.6, puppet3.0 rc in final version, puppetdb 1.0, mcollective 2.2

puppet 3.0:much faster.TONS of bug fixes. 
puppet certs started

software defined infrastructure is the future

test pilot program (registered for it) for active feedback and early release access for testing/input

pupetdb applications! storedconfigs written in 2006, not really as fast. more functions and applications built on top of it are coming (rc_whatis= super early basic example)

Keynote #2: Paul Strong, VMware
-------------------------------
CTO, Global Field, Vmware
From Virtual Server to Software Defined Data Centers: On going Evolution of IT

Software: from monolithic applications -> distributed services
Platform: from monolithic, loosly connected discrete resources -> Virtualized fabric of resources

Cloud = new consumption model for IT. No barriers to entry for building new businesses, not need to for upfront capital expenditures for startups

automate the hell out of everything, cost model flips and energy becomes your biggest cost and what to optimize around(utilization, data center location etc) (We are semi-here?)

thinks we're living in the most exciting time in IT. it sucks, but sucks way less than it used to. 

Keynote #3: Gene Kim, founder of tripwire, author of Devops cookbook,when IT fails
----------------------------------------------------------------------------------

IT Sucks: 4 acts: 
1: IT ops supporting fragile apps/etc, start missing commitments to outside world.
2: Product Managers: when commitments are missed, larger commitments are made to make up for it. unachievable requirements.
3: Developers: Unmovable deadlines, corners cut, more shortcuts, more fragile code/applications. = Technical Debt. 
4: Dev and IT ops at war. Dev has it working @ workstation/test, tons of work for ops to make it go across infra. Project work starved, bussiness+ops_personal problems. 

All orgs are pressures to simultaneously respond more and more quickly to critical issues, while developing an increasing stable infrastructure. almost mutually exclusive

better way?

ops think like devs, devs think like ops

dev->ops: understand flow of work, seek in crease flow, never pass defects down, no local optimizations, profound understanding of system.
Redefine Types of work: business projects, internal projects, changes, unplanned work.
Create one stop env creation process: build code and env @ same time, code in dev is running in same platform it will end up in! 
outcomes: single repo for code and environments, determinism in release process, consistent envs, faster releases. 

ops->dev: understand customer needs, internal and external. shorten and amplify all feedback loops,quality @ the source. 
Embed Devs into IT ops, escalation process and root cause meetings, cross train devs.
outcomes: fix issues faster, detect faster. devs know what ops resources are needed for a given task, better communication, more work done. 

dev <-> ops : continual learning, forced experimentation, learn from failures, ability to retreat back to safe areas from "danger zone". repetition.

inject failure often: only way to survive failure is to fail all the time. Netflix Chaos monkey: randomly kill services/systems in production so everything must be resilient! you don't get to choose chaos monkey, chaos monkey chooses you! 

break things pre-prod! 

allocate 20% of cycles to Tech. Debt reduction! Otherwise, you will have 100% of cycles spent in debt reduction

~ 3 trillion $$ spent on IT failures!


Better Living through Statistics: Why Monitoring (Doesn't have to) Suck!
------------------------------------------------------------------------

Jamie Wilkinson
Google Australia - Systems Engineer

What is monitoring? Measuring recording, alerting, visualize
Automate the boring parts

current state: blackbox results in alerts. Whitebox results in charts. 

Blackbox: Nagios fire thousands of check scripts! fires alerts. maybe write some data to TSDB. 
No predictive. Binary (ok or failed)

Whitebox: Looking at ganglia/other TSDB. Tried to predict...maybe? monitor changes, alert if/when needed, not all the time

Difficult: Different thresholds, hard to tune, adding new stuff is hard. check script performs teh check and logic all in one. Alerts for things you can't act on. duplicates.

Alerting on rate of change: if speed > 50mph, bus armed, if speed <50mph, bus explodes. but Keanu only care if bus <50mpg. But REALLY only should care when acceleration goes bellow 0, ie, danger zone. Calculus! Detect rates! Determine baseline of errors.

Don't just look at most recent value, but look back 5 minutes, 1 day, 7 days, big bang, etc. Spikes get muted, not worth getting out of bed for. longer spikes? maybe. Example from RC: High CPU, maybe just user +updates+puppet. DO we need an alert right away? or only if its sustained? 

aggregate parts of infrastructure in ganglia, clusters, not just looking at each host...

look @ ratios : errors  and requests :are we serving mostly errors? 

What to measure: queries per second, errors per second, latency, bandwidth. 

don't alert on load avg? 

alerts != logs, make sure its actionable, then document it. 3-month-future will thank you. 

blackbox tests are end-to-end, still needed. 

How? R, numpy, etc! Demo time

Thoughts/Take aways: We need WAY better monitoring, and more though about what we want to monitor, how and why, not just blindly monitoring everything. We had nothing, now we check too much. Need the team to come up with what we want, whats important. The talk was...okay. Good info/thoughts, mostly focused around the math needed to create all your own metrics/thresholds for things like web page requests/failures. in GO. We should keep seeing if hosts are up but dial back load metrics. Maybe start checking ganglia again? (how automatically? different ports, Chang had this hardcoded...)Run reports based on historical data (ganglia) to alert on things (X host tends to be over loaded often, etc).


Puppet @ Git Hub
----------------

Jesse Newland
GitHub - Ops

THE SETUP = githubs internal tool fo puppet drive latop setups! Talk tomorrow.

Puppet Infra @ github.com, 4 years since move to rackspace. 100K likes code

Keep deployment simple. Single puppet master
puppet does not run in agent mode! 

no ENC, but DNS(FQDN) names map systems to roles (app1 = appserver, db1=db server, etc)

Heavy use of augeas (we need to do this) to manage PARTS of config files not the whole thing! 

Whats interesting about puppet @ github?
Ensure puppet dev workload is easy to access for everyone. Make it less scary for those unfamiliar with it

heavy use of forge modules (librarian)

Tests! makes people lees scared. Safety net. rspec-puppet (need to use this) use to test roles, test critical functionality. 

Pre-commit hooks (we have this, well, pre-push, you can commit bad things all you want)

https://github.com/github/hubot

HUBOT: Capistrano used to experiment with branches (environments) in CI. used to speed deployments faster than corn would roll it out. ask for logs to make sure you didn't break things. 
HUBOT = star of the ops team, used to pull together lots of direct things they've written via json apis. hutbot 
talks in chat, so all users see all activity, active learning for everyone from day 1!
teaching by doing by making things visible. 
chatops

Things I haven't asked recently: 
how is that deploy going? 
has anyone responded to that alert? 
has anyone updated the status page? 
who is working remote? 

iphone client! 

and now for something completely different: 
uses pagerduty.com alerts, so you can ack alerts from your phone! (we should checkout pager duty, they have a booth here as well, i'll check out)

Thoughts/Take aways: hutbot sounds awesome. like rc_whatis but on acid to DO things not just SHOW things. love the idea of things going to chat, new guy comes on and sees all sorts of things happening. everyone sees Matt deploy some new puppet bits, or a DNS change, etc. like the idea of dns dns being used as the classification for what a system looks like, doubtful we could fit that in however. They DO use a pre run stage. only 200 nodes soa  single master makes sense. 
 
External Data
-------------
Kelsey Hightower PuppetLabs Ops Team Lead

What is puppet data:

* class params + variables
* classification
* reports
* storedconfigs

Openssh examples module:
"Data" = variables at first, inside the module, install pkg, write config using template w/ the variables...
changing data = edit you manifest (adding a user)Cool? eh...but what if the module changes, but not your data? Too much work do one thing.
What if you have a big selector? Also meh. Make data cry. (picture of data from star trek weeping)
Better = Pararmeterized Classes. Move variables into class whatever(HERE ){ }, suddenly those become defaults that can be overridden.  We do this for some stuff. Data is out of the module, but in nodes.pp/site.pp...
Better as people can just edit site.pp nodes.pp, and so long as the public interface to the module stays the same, it just works...

Once you have a parameterized class, you can start using Hiera

Hiera: 1.0, included with 3.0, but available for 2.7. Transparent class parameter lookups...
Dynamic Data, Hierachical Data store
Pluggable Backends (json, yaml included, could use mysql)
Lookup Functions
Read-only

Yaml backend:

	---
	allowusers: ["user1","user2"]
	banner: '/etc/issue.net'
	

	$allowusers=hiera('allowusers',[])

that would look up the users, and use the empty for the default.

/etc/puppet/hiera.yaml

can have multiple backends, default yaml, walks backends until it finds an answer. 

specify backends, backends have a data dir, specify a hierarchy. can use facter substitution. 


put hiera functions in the parameterized class instead of defaults, to try to lookup things, provide defaults otherwise. 

next is the ENC then
ENC: responsible for classification, class parameters, global variables (bad, but we do this) environment.

builtin enc:
nodes.pp (ours)
yaml
exec
ldap

for exec enc, just pass hostname (fqdn), and send back yaml. cobbler, foreman, puppet dashboard

data bindings
puppet 3.0, using hiera, automatic lookup for parameterized classes, namespaced variables

resolution order: user supplied, hiera, class defaults. hiera lookup is transparent. 
means in hiera/enc, but fully name space vars (this is our biggest hurdle to get to 3.0, basically just lots of variable name changes)

Thoughts/Take aways: Hiera seems great. I like ENC's but I've got questions about them + hiera about groupping/ fuzzy matches, like heroXXXX getting some value. Talk ran long so hoping to find speaker in hallway later to ask


Higher-Order Puppet: PuppetDB
-----------------------------
Deepak Giridharagopal
Puppet Labs - Director of Engineering

@grim_radical

What can your data DO for you? 

puppet generates lots of data. 

Resources: a thing. yep. a simple file{/tmp/foo: ensure => present} is a resource, compiled into much more data.
Catalog: all a nodes resources, and how they relate to each other. What we tell puppet about a node
Facts: facter, duh. What a node tells puppet about itself. 

"Its all about who controls the information"

"Juicy Data Puppet has":
every resource of every catalog with every fact for every node 

what if you can feed this data back into puppet itself: storedconfigs. nagios = our biggest example. and ssh keys.

could be used for: firewall rules, clustering, replication, load balancers. Anything where info from 1 node is needed to determine how to configure another node. 

Use puppets knowledge to improve puppets knowledge. 

When the storage for catalogs is slow, everything slows down. API issues too. 

Store all the data we can, plus make it as queriable as possible, but don't slow things down! 

PuppetDB = Puppet Defiantly Better. ;-)

science and secret alien tech. 

puppetdb server: imbedded httpd server = all access, message queue, db, worker threads, dlo. 

all info processing on puppetdb happens at the same time the master+agent are working, shouldn't really block. 

on failures, if things cannot be inserted, go to DLO, Dead Letter Office. Logged byte for byte copy of what failed to go in. 

To balance, have multiple puppetdb's (one DB), with a http proxy in front just like puppet load balancing

can brenchmark directly (use this for testing new puppetdb server when time comes, you can simulate X nodes checking in at Y rate with some random in there for changes)

query reosurces, push facts by hand (not from nodes facter runs...)

github module to do more complicated queries within puppet manifests
nagios checks for puppetdb

hosts are no 100% unique, so, we can store a resource only once if its not different. 
same with catalogs. 

basically, de-dup! 

no locks. , very compact code 60% (), good docs. 

and a screenshot of out pupeptdb dashboard showing our 3 million+ resources was just showcased at the end. 

Whats New in Puppet 3.0
-----------------------
Eric Sorenson
Puppet - Open-Source Product Owner

Telly: Early Sesame Street character addicted to TV. Character was re-written to not promote sitting 2 feet from TV. Lovable guy w/ some issues

Also, code name for 3.0 release. 

3.0 should be out any day now (last minute webrick problems)

shiny bits @ release time: 

* pluginsync improvements 
	* enabled by default (yay, better boot strapping!)
	* plugins are lazy loaded. loaded as needed instead of when downloaded+at the start
	* splits out sync'ing faces etc
	* faces come from gems now
* Platform Support
	* Ruby 1.9.3 support!
	* Solaris Support for 10 and 11 (SMF and zones)
	* Windows msi package provider
* Hiera Supper
	* automatic lookups! 
* Language Features
	* unless
* Performance increases (BIG)
	* less variability, faster 
	
* Ruby
	* no 1.8.5 support (provisions runs 1.8.5, salves run newer)
	* upgrade path: puppetlabs provides 1.8.7 EL6 backports! yay!
	
* dynamic scoping gone (this will need some re-writes!)

* Future:
	* better quality
	* slimmer, split out some core types like nagios providers into modules
	* Themed releases in 3.X, cycles.
	* Driven by User Testing.

Reading from the Puppet Data Library
------------------------------------
Nick Lewis, Puppet Labs - Core Developer

Puppet knows your infra.
And it thinks. 

more what less how. 

speaks data

Puppet Data Library: more of a principle, automatically collected, accessible by YOU, everything puppet knows.
PuppetDB = the bookshelves

* Today:
	* Facts 
	* Catalogs
* Soon: 
	* Events
	* Historical data

**Big Data**
its not the size of the data you have, its what you do with it

This is **not big data**! 

See your data, form an emotional and intellectual connection with it. 

Make things from your data, audit relationships.



Day 2, friday Sept 28th
=======================
Key Note #1:
------------
Dan Hushon, EMC Corporation - Distinguished Engineer

Confounding Complexity

k2:
1000 node, 24,000 core clustsr, 24PB, hadoop cluster for testing

how to manage very large, very complex systems
independence, link independent systems in to hierarchies, partition

Razor provisioning Windows demo! 

Key Note #2
-----------
Tim Bell, CERN - Operating System and Infrastructure Manager

Accelerating Science with Puppet

Answering fundamental questions: mass, what is 96% of the universe, why not anti-mater, etc etc

Lots of cool technical descriptions of the ring etc.

1PB/sec generated

Computing:
couple 100+ node farms to just do initial filtering of raw raw data. 
tier 0, tier 1, tier 2
25PB /yea to record, 20 year retention. 
6GB/sec normal, 25GB/sec peak.
45,000 tapes, holding 73PB of data. 

20% writes, 80% read

New data center need for energy, additional 2.7MW (currently @ 3.5MW)
hands off site, managed remote 

New strategy: not special anymore, look to others for ideas. 

pets + cattle:

pets are given uniqe names, you care about them, you fix them when sick
cattle are given numbers, when sick you shoot them

lots of forge modules, use foreman for proivisioning + openstack

LHC@home boinc project, puppet module available. 

next steps: puppetdb, mcollective
15K hypervisors, 100,000-300,0000 virtual machines
github cernops


Key Note #3
-----------
Joachim Thuau, SpaceX - System Engineer - Linux

how to stay sane while sending rockets into space

only recent puppet adoption

ubuntu/debian based evn
AD/Ldap
local apt mirror + custom repos
lots of scripts...
pxe boot to build, puppet is installed, does the rest of the config. 
foreman used. 

awesome vids of falcon 1, 9, and simulation of falcon heavy. 

Package management and Puppet
-----------------------------
Sam Kottler
Red Hat - Software Engineer

homebrew package provider, also macports + fink for osx

module_name/plugins/puppet/provider/package/provider.rb for custom bits

arrays and methods really. 

fpm -s gem -t rpm rails (builds rpm for rails from gem!)

Razor : DevOps + Bare-metal Provisioning 
----------------------------------------
Nicholas Weaver
VMware - Automation Architect

provisioning sucks
lots of tools, all kinda work, each its own way, some os's but not others..meh, not devops
scale

cloud/virt can go forever, physical is finite

in a dream world:
treat things like they ARE virtual
declare what it should be. 
slice dice as needed
link into the real configuration

puppet is really good at config...occams razor

razor suppers: sles, opensuse, RHEL, centos Esxi, debian, ubuntu, more (windows soon)

5 major things to setup and deploy w/ razor:

discovery: razor does not do dhcp, you pxe systems into a razor microkernel. this contains facter! then it sits in ipxe basically. razor then says hello and registers it w/ facts etc. 
tagging: tags facts basically, so you can look at discovered systems in groups like "virtual", "C6100s" etc. User defined too. match tags too. 
modeling: model templates = supported os's, base definition. + metadate = model. has enough to get system up then let puppet take over. assign hostname prefix and domainname
brokers: broker = puppetmaster
policies: tags + model +broker(optional), work like firewall rules tables, top down eval, exists on a rule 

coming soon: new api + cli updates, vmware integration, windows provisioning (no wds, opensource,just like *nix deployments)

https://github.com/puppetlabs/Razor

Getting Started with Contributing to Puppet and Facter
------------------------------------------------------
 Ruth Linehan, Tesca Fitzgerald, Hailee Kenney

 Not scary! 
 
 Why do it? Its nice to do! Help add your own features, etc. IRC Karma! 
 
 Open a redmine account, links.puppetlabs.com/patches_welcome 
 
 always fork, clone, edit! 
 
 write spec tests! 
 
Logging: logstash and other things
-----------------------------------
Jordan Sissel
DreamHost - czar of logging

Runs masterless puppet! Also runs nodeless! 

makes fpm! 

disk full ? rm -rf ALL LOGS, we don't need that shit! 

what else sucks? Shitty error messages
logs: no std format, generally we have some every + tiemstamp though
normal way: send to syslog, rotate, throw away later.

logstash! 

configs look like puppet. like, really like puppet. 

write patterns for logs of various logs, simple, suddenly you have context around logs

regex? Grok! 100 patterns+ can be extended. no need to write yuor own regex's for each thing, just use the "ip" grok `%{SYNTAX:name:type}`

too many time formats. logstash has a date filter to fix. 

doesn't process by lines, so multi line events can be filtered as events, not just lines!

can output to ganglia, etc

Foreman and Friends
-------------------
Ohad Levy
redhat - Principal Software Engineer

Realm: Provision everything, config mgmt (puppet), inventory, active reporting, one simple ui.
can do bare metal provisioning, as well as RHEV-M/ec2,openstack (kvm?)
smart proxies: use existing infrastructure (dns,dhcp,etc), allow foreman to use them over its apis.
 
provisioning demo, creating a new host
1.1 coming soon (need to upgrade to 1.0!)
support for parameterized classes

smart proxies look cool, we could use more of these features

all templates for provisioning are erb based. (like THIS better than cobbler), full audit log
whoa, in browser vnc for console to your systems!
organization and location support coming! 

Config managment @ CERN
-------------
Ben Jones,CERN - Devops

12K nodes

wrote own cfg mgmt, Quattor.

foreman used for provisioning, looking at razor. love the foreman api. 
pre-existing infra = hyperV, new stuff all openstack puppeted

load balanced puppet, multiple foreman servers w/ memcached, separate ca server(bugs)

all git, branches = environments like us, prod dev test branches, topic branches for major changes

share modules on github, cernops

cernit not only puppet deployment on site, atlas does their own. 

use hirea, and gpg for secrets. db backend for integration with other systems (monitoring, metrics etc)

lots of modules form the forge etc used. 

some open issues, solves diversity, but multiple admins? rolling updates

The END! 





