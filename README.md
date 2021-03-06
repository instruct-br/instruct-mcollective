# Mcollective

#### Tabela de conteudo

1. [Overview](#overview)
2. [Compatibilidade](#compatibilidade)
3. [Setup](#setup)
4. [Uso](#uso)
5. [Limites](#limites)
6. [Pendende](#pendente)
7. [Referencias](#referencias)

## Overview

Esse módulo configura o Mcollective Server e Cliente, ambos usando SSL para se conectar
ao ActiveMQ. O módulo também instala plugins Package/Service/Puppet e gerencia o arquivo
`facts.yml`.

## Compatibilidade

Esse módulo é compatível apenas com o MCollective do Puppet Agent >= 1.2.2 (Puppet 4.2).

Esse módulo não instala o MCollective, pois no Puppet 4 ele já vem embutido no pacote Puppet Agent, logo ele gerencia apenas configurações.

Esse módulo foi testado nos seguintes sistemas:

* CentOS 5, 6 e 7
* Debian 6, 7 e 8
* Ubuntu 12.04 e 14.04
* SLES 11 e 12

## Setup

### O que esse modulo gerencia?

#### classe mcollective::server

* arquivo server.cfg
* certificados publico e privado do server
  * plugin.ssl_server_private
  * plugin.ssl_server_public

#### classe mcollective::client

* arquivo client.cfg
* certificados publico e privado do client
  * plugin.ssl_client_private
  * plugin.ssl_client_public

#### classe mcollecive::plugins

* mcollective plugins
  * package
  * service
  * puppet

#### classe mcollective::facts

* gerencia o arquivo de fatos
  * facts.yaml
  * tarefa para gerar facts.yaml

## Uso

### declarando classe mcollective de forma mínima

```puppet
  class { ::mcollective:
    use_server                    => true,
    use_client                    => false,
    activemq_pool_host            => '192.168.200.80',
    activemq_pool_port            => '61614',
    activemq_pool_user            => 'mcollective',
    activemq_pool_password        => 'marionette',
  }
```

### declarando classe mcollective com todos os argumentos permitidos

```puppet
  class { ::mcollective::server:
    use_server                    => true,
    use_client                    => false,
    activemq_pool_host            => '192.168.200.80',
    activemq_pool_port            => '61614',
    activemq_pool_user            => 'mcollective',
    activemq_pool_password        => 'password',
    mco_main_collective           => 'mcollective',
    mco_collectives               => 'mcollective',
    mco_loglevel                  => 'info',
    activemq_pool_ssl_ca          => '/etc/puppetlabs/puppet/ssl/ca/ca_crt.pem',
    activemq_pool_ssl_key         => '/etc/puppetlabs/puppet/ssl/private_keys/node.pem',
    activemq_pool_ssl_cert        => '/etc/puppetlabs/puppet/ssl/certs/node.pem',
    mco_plugin_yaml               => '/etc/puppetlabs/mcollective/facts.yaml',
    mco_registerinterval          => '300',
    mco_service_name              => 'mcollective',
    mco_server_public             => '/etc/puppetlabs/mcollective/ssl/server.crt',
    mco_server_private            => '/etc/puppetlabs/mcollective/ssl/server.key',
    mco_client_cerdir             => '/etc/puppetlabs/mcollective/ssl/clients',
    mco_client_public             => '/etc/puppetlabs/mcollective/ssl/clients/client.crt'
    mco_client_private            => '/etc/puppetlabs/mcollective/ssl/clients/client.key'
 }
```

### declarando classe mcollective::server

```puppet
  class { ::mcollective::server:
    activemq_pool_host            => '192.168.200.80',
    activemq_pool_port            => '61614',
    activemq_pool_user            => 'mcollective',
    activemq_pool_password        => 'password',
    mco_main_collective           => 'mcollective',
    mco_collectives               => 'mcollective',
    mco_loglevel                  => 'info',
    activemq_pool_ssl_ca          => '/etc/puppetlabs/puppet/ssl/ca/ca_crt.pem',
    activemq_pool_ssl_key         => '/etc/puppetlabs/puppet/ssl/private_keys/node.pem',
    activemq_pool_ssl_cert        => '/etc/puppetlabs/puppet/ssl/certs/node.pem',
    mco_plugin_yaml               => '/etc/puppetlabs/mcollective/facts.yaml',
    mco_registerinterval          => '300',
    mco_service_name              => 'mcollective',
    mco_server_public             => '/etc/puppetlabs/mcollective/ssl/server.crt',
    mco_server_private            => '/etc/puppetlabs/mcollective/ssl/server.key',
    mco_client_cerdir             => '/etc/puppetlabs/mcollective/ssl/clients',
 }
```

### declarando classe mcollective::client

```puppet
  class { ::mcollective::client:
    activemq_pool_host            => '192.168.200.80',
    activemq_pool_port            => '61614',
    activemq_pool_user            => 'mcollective',
    activemq_pool_password        => 'password',
    mco_main_collective           => 'mcollective',
    mco_collectives               => 'mcollective',
    mco_loglevel                  => 'info',
    activemq_pool_ssl_ca          => '/etc/puppetlabs/puppet/ssl/ca/ca_crt.pem',
    activemq_pool_ssl_key         => '/etc/puppetlabs/puppet/ssl/private_keys/node.pem',
    activemq_pool_ssl_cert        => '/etc/puppetlabs/puppet/ssl/certs/node.pem',
    mco_plugin_yaml               => '/etc/puppetlabs/mcollective/facts.yaml',
    mco_registerinterval          => '300',
    mco_service_name              => 'mcollective',
    mco_server_public             => '/etc/puppetlabs/mcollective/ssl/server.crt',
    mco_client_public             => '/etc/puppetlabs/mcollective/ssl/clients/client.crt'
    mco_client_private            => '/etc/puppetlabs/mcollective/ssl/clients/client.key'
  }
```

### declarando classe mcollective::facts

```puppet
  class { ::mcollective::facts:
    mco_plugin_yaml => '/etc/puppetlabs/mcollective/facts.yaml',
  }
```

### declarando classe mcollective::plugins

```puppet
  class { ::mcollective::plugins:
    mco_plugindir => '/etc/puppetlabs/mcollective/plugins',
  }
```

## Limites

1. Esse módulo não gera os certificados ssl tipo server (plugin.ssl_server_private|plugin.ssl_server_public), você precisa gerá-los e colocá-los no diretório mcollective/files/ssl

2. Esse módulo não gera os certificados ssl tipo client (plugin.ssl_client_private|plugin.ssl_client_private), você precisa gerá-los e colocá-los no diretório mcollective/files/ssl

3. Esse módulo só suporta a conexão do tipo SSL, não há suporte a PSK para securityprovider.

4. Esse módulo só suporta o ActiveMQ como connector

5. Esse módulo só instala os plugins Package, Service e Puppet (arquivos do PE 2015.2)

6. Esse módulo não instala o ActiveMQ

## Pendente

Homologar:

* Windows Server 2008 e 2012

## Referencias

http://gutocarvalho.net/octopress/2015/09/09/activemq-e-mcollective-com-ssl/
