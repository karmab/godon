# Godon Test Run Coordinator

## instanciate

> Instanciate Test Infra Stack with help of kcli on local libvirtd

~~~sh

set -eEux

echo "instanciating infra"

kcli create host kvm -H 127.0.0.1 local

echo "generating instances credentials"
ssh-keygen -N "" -q -f ~/.ssh/id_rsa 

kcli create plan -f "${SOFT_RESIDE}/infra/pools.yml" pools 
kcli list pool
kcli download image -p image_pool fedora30

kcli create plan -f "${SOFT_RESIDE}/infra/machines.yml" micro_fedora
kcli list image
kcli -d start plan micro_fedora

sleep 1

kcli list plan
kcli list vm

~~~

## provision

> Provision Test Infra Instances

~~~sh

set -eEux

echo "provisioning infra"
ansible-playbook -i ${SOFT_RESIDE}/klist.py \
                 -l sink_vm ${SOFT_RESIDE}/automation/setup.yml
ansible-playbook -i ${SOFT_RESIDE}/klist.py \
                 -l source_vm ${SOFT_RESIDE}/automation/setup.yml
~~~

## perform

> Perform/Orchestrate Run of Tests 

~~~sh

set -eEux

echo "performing  tests"
# ansible to come
~~~
