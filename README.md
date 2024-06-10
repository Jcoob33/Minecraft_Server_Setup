## Requirements For Pipeline

### Needed Tools

- **Terraform** (v1.0+)
  - Installation:
    - **Windows**: Download from [Terraform Downloads](https://www.terraform.io/downloads.html).
    - **macOS**: Use Homebrew: `brew tap hashicorp/tap && brew install hashicorp/tap/terraform`.
    - **Linux**: Use the package manager or download from [Terraform Downloads](https://www.terraform.io/downloads.html).

- **Ansible** (v2.9+)
  - Installation:
    - **Windows**: Use WSL and follow Linux instructions.
    - **macOS**: Use Homebrew: `brew install ansible`.
    - **Linux**: Use the package manager: `sudo apt update && sudo apt install ansible`.

- **AWS CLI** (v2)
  - Installation:
    - **Windows**: Download from [AWS CLI Installer](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).
    - **macOS**: Use Homebrew: `brew install awscli`.
    - **Linux**: Use the package manager: `sudo apt install awscli`.
  - Configure: `aws configure`.

- **SSH Client**
  - Installation:
    - **Windows**: Use the built-in OpenSSH client or install [PuTTY](https://www.putty.org/).
    - **macOS and Linux**: SSH client is usually pre-installed.

- **Docker**
  - Installation:
    - **Windows and macOS**: Download and install from [Docker Desktop](https://www.docker.com/products/docker-desktop).
    - **Linux**: Use the package manager: `sudo apt update && sudo apt install docker.io`.

### Configuration

- **AWS Credentials**: Configure your AWS CLI with valid credentials using `aws configure`.
- **Ensure SSH Key Permissions**: Set the correct permissions for the SSH private key: `chmod 400 terraform/minecraft-key.pem`.

## Diagram of the Major Steps in the Pipeline
  -First all the depencies need to be downloaded after that the terrraform script can be ran to set up the ec2 instance. Following the succesful setup of the ec2 instance the playbook can be ran to configure the minecraft server. Once the server is configured all there is left to do is to connect.

```mermaid
graph TD;
    A[User Setup] --> B[Terraform Apply]
    B --> C[Provision AWS Resources]
    C --> D[Run Ansible Playbook]
    D --> E[Configure and Start Minecraft Server]
    E --> F[Access Minecraft Server]
```
*** Pipeline steps exaplianed/Needed commands
***Setup: Like in the diagram above the fist step is going to be ensureing that you have all the nessery dependenices downloaded onto your machine.
These dependenices can be found above with links for macOS, windows, and linux. Follow the links and download the one corrisponding to your 
machine. Once the dependinces are downloaded you can copy the files in the directoy to your local machine to be used in the following steps.

***Terraform: First we need to make an ec2 instance which can be done through the provisioning script in the file Main.tf. First open a terminal
and use the cd command to get into the "Terraform" folder. Once you are in the folder yur are then going to run.
```sh
    terraform init
```
This command initalizes the working directory containing the terraform configuration files. After that we are going to run the following command from the same terminal.
```sh
    terraform apply
```
The "terraform apply" command is going to apply the configuration sprict and create the ec2 instance with the correct configureations. After framework is made the ec2 instance will be running which brings us to the next step

***Ansible(Minecraft server setup): Now that the ec2 instance is running we now just need to setup and configure the minecraft server. The server will be run useing docker with a docker image, the ansible script will download the needed dependences. To run the playbook your are going to perform the following command from the terraform folder that you should curently be in.
```sh
    ansible-playbook -i inventory.ini Minecraft_setup.yml
```
After that command is ran you should be able to see all the section being setup through the terminal, once it is finished and you have access to the terminal you should be able to access the server.

***Accessing the server: Now that the server is up and running you can check it through two methonds

### Resources

#### Ansible:
- [Ansible User Module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html)
- [Docker Minecraft Server](https://docker-minecraft-server.readthedocs.io/en/latest/#using-docker-compose)
- [Ansible Service Module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/service_module.html)
- [Ansible Yum Module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/yum_module.html)
- [Ansible Docker Container Module](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_container_module.html)
- [Docker Minecraft Server Examples](https://github.com/itzg/docker-minecraft-server/tree/master/examples)

#### Terraform:
- [Terraform AWS Provider Documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Terraform EC2 Instance](https://spacelift.io/blog/terraform-ec2-instance)
- [Terraform Internet Gateway Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/internet_gateway)
- [Terraform Route Table Association Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route_table_association)
