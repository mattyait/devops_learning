
# What is Configuration tool?
Configuration tool is the tool responsible to install sopftware or maintain the existing software on machine in a idempotent mode.
In more simple language, If you want to install nginx server or appache server along with it's configuration files then you can use a configuration tool to do the setup and it will make that configuration as code for future versioning purpose.

## Comparison of Configuration tool
There are various comparison between various configuration tool like Chef vs Puppet vs ansible vs terrform ( terraform is not a configuration tool its a orchestration/provisioning tool and Terraform enables any configuration management tool)
If you want to create machines on AWS cloud or on premises data center then terraform comes in picture not for setting up the software, although some configuration tools also provide provisioning part like Chef and Ansible both based on "resource" (for chef) or "module" (for ansible) availability.

# Chef vs Ansible
I have worked on both chef and ansible configuration tool and I found chef tool is a mature and stronger tool than ansible.
Chef wotk on complete practice of keeping everything as code, therefore in addition to automating infrastructure with configuration management, you can keep security compliances also as a code for your infrastructure.
Ansbile is very easy to start that the reason it's becoming very popular and there is no programming knowledge needed in case if you are not writing your our ansible module which is rare in many cases.
It has symple yaml syntax which can be understand by system admin people therefore it's famous among people. In my opinion if you liking a tool because of the reason of no programming requirement then that tool is not for devops its for system admin guys else it will miss the dev part from the devops.

## Scalable performance
Ansible provides an agentless execution model that makes it easy to get started without the need to manage additional software. 
Ansible compiles specified playbooks which it then sends to each node via SSH for execution. This direct execution model means the more nodes you manage, 
the more performance degrades. At small scale, the performance impact is manageable and typically unnoticeable. 
In distributed production environments, that linear performance leads to exponential failures once system performance thresholds are reached.

Chef relies on installing an agent (the “chef-client” binary) on each managed node. Each chef-client takes on the brunt of managing any performance impacting operations locally.
By distributing this load across every machine in your fleet, Chef makes it easy to manage tens of thousands of servers without suffering from performance degradation.

## Complexity

Ansible’s focus on simplicity makes it a good match for organizations with limited skill sets and a small infrastructure footprint. 
Ansible abstracts configuration into YAML modules called ‘playbooks’.

Chef has a domain specific language (DSL) that provides an easy to approach interface for managing basic configuration tasks.
For complex scenarios, users can leverage the full power of the Ruby programming language to model appropriate solutions
You don’t need to know Ruby to use Chef, but when facing uncommon edge case scenarios it’s easy to use just enough Ruby to work your way through the most complicated tasks.

## Why Chef?
Chef is complex, but it's really fast and powerful for a scaled infrastructure I'm talking about handling infrastructure in multiple region.
Chef uses chef server and node concept and node needs to be registered with chef server for authenticated and running cookbook to configure the machine.
It all happens over Chef Server API which is REST API so there is no need to have ssh port open, However on the other side ansible need to have ssh port open for machine and think of scalable infrastrucure distributed across multiple regions to open the ssh ports.
An SSL certificate is used between the chef-client and the Chef server to ensure that each node has access to the right data
Every request made by the chef-client to the Chef server must be an authenticated request using the Chef server API and a private key. 
When the chef-client makes a request to the Chef server, the chef-client authenticates each request using a private key located in /etc/chef/client.pem.

Below diagram will give the basic idea to handle the scaled infrastructure via Chef and Ansible


All communication with the Chef server must be authenticated using the Chef server API, which is a REST API that allows requests to be made to the Chef server


I have worked on both chef as well as ansbile configuration management tool and sometime I have seen people saying ansible as best without having experience of working on other tool.
I think ansible is easy and that should not be the reason to say it as best, each tool has different purpose and requirement. Sometime you need to have great vision to implement and manage the robust infrastructure
otherwise you will have to rewrite/redesign in future at some stage. for example netflix and Uber has redisgned their complete solution to achive the right growth, At initial stage they might have thought they are doing it in best way :)


- Best thing in Chef (Roles and environment)
You will have different cookbook version and each version is different configuration stage. So in chef environment will point to specific version of cookbook and in case of rolling out the new version you just have to point the environment to specific version of cookbook and environment would be reset to that configuration.
