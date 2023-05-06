# Podman Quadlet 演示

使用Podman的Quadelet和Systemd来模仿Kubernetes示例：[使用持久卷部署WordPress和MySQL](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/)。  

## 使用Ansible部署示例

- 启动目标机器
    - 该演示在[openSUSE Tumbleweed](https://get.opensuse.org/tumbleweed/)通过测试。  
- 在开发机器上:
    - 安装Ansible
        ```
        sudo zypper in ansible
        ```
    - 克隆该代码库
        ```
        git clone -b vg-suse https://github.com/mengzyou/quadlet-demo.git
        cd quadlet-demo/
        ```
    - 安装依赖的Galaxy collections
        ```
        ansible-galaxy collection install -r requirements.yml
        ```
    - 创建Ansible主机配置文件 - `inventory.yml`:
        ```yaml
        all:
          hosts:
            < FQDN or IP of the target machine >:
              ansible_connection: ssh
              ansible_user: < Username to connect with >
              ansible_ssh_private_key_file: < Location of the private key >
        ```
    - 运行playbook
        ```
        ansible-playbook -i inventory.yml playbook.yml
        ```
    - Open the machine's URL in a browser, using port 8000 and `https://`
      and you should see the Wordpress setup page.
    - 浏览器访问 `https://<IP of target machine>:8000/`，你将会看到Wordpress的安装配置页面。  

## 使用Vagrant和libvirt部署

This repository contains a Vagrantfile, that can be used to create a new
virtual machine and deploy the quadlet-demo using Ansible.
可以使用代码库里的 *Vagrantfile* ，使用vagrant创建一个libvirt类型的虚拟机（[opensuse/Tumbleweed.x86_64](https://app.vagrantup.com/opensuse/boxes/Tumbleweed.x86_64)的Box），然后直接通过ansible的插件直接部署该示例。  

- 需要 vagrant 和 vagrant-libvirt
  ```
  sudo zypper in vagrant vagrant-libvirt
  ```
- 获取 `opensuse/Tumbleweed.x86_64` box的镜像  
  ```
  vagrant box add opensuse/Tumbleweed.x86_64
  ```
- 执行 `vagrant up`

运行完成之后，可以通过文件 `.vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory` 查看虚拟机的IP地址，然后浏览器访问 `https://<ip of vm>:8000/` 。  
