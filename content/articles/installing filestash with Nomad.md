Installing Filestash with the Nomad orchestrator
2020-05-24
Technology

## Installing Filestash with the Nomad orchestrator

Filestash is a very low consumption app so if you are using a big-ish server it makes sense to run it together with other applications. Nomad is an easy way to orchestrate containers and applications in a single server without having to deal with the complexity of Kubernetes.

For this simpler use case Nomad is an alternative to docker-compose and docker swarm. Following this documentation it is possible to setup a fully production ready instance of Filestash with a battle tested orchestrator.

### Getting ready

You will need the following software installs:

* Nomad https://www.nomadproject.io/downloads/
* Docker
* Nginx

Then you will need to run the following services as Nomad jobs:

* Consul
* Fabiolb

I am assuming a bare metal or dedicated server setup. I am running an Ubuntu OS but because Nomad runs with a single binary all Linux OS systems should be supported.

### Nomad Web UI

It is better to get started using the Nomad Web UI to run jobs as it is easier to get started this way.

If you have installed Nomad correctly on your server you will find Nomad on localhost:4646 or yourip:4646. Make sure Nginx is not running otherwise you need to map Nomad web UI address to a Nginx location.

Here is how the Nomad UI should look like.

![nomad web ui](https://i.ibb.co/71C2GCJ/Capture.png)

You can run all Nomad jobs below using the UI. Just click on the button `Run Job` in the top right corner.

This is how the web UI looks like for running jobs. You paste the job and click `plan` (bottom left), then `run`.

![nomad web ui jobs](https://i.ibb.co/XyytSdp/Capture.png)


### Getting familiar with Nomad, Consul and Fabio

If you are not familiar with Nomad I suggest to start from their Learn section: https://learn.hashicorp.com/nomad

After having learned Nomad you can proceed with the below installations. You don't need to understand Consul deeply to do this. Fabio is quite simple and it works as a load balancer. Fabio is needed here so you don't need to manually map each ip address of each app instance to a server. Fabio replaces Nginx in this setup.

### Setting up Consul in Nomad

There are various ways to run consul. Since Nomad supports running binaries as services (while Kubernetes only runs with Docker containers), we can run Consul from the Consul binary using the job below.

Note the driver here is `raw_exec`, if we were running a docker container the driver would be `docker`.

```
job "consul" {
  datacenters = ["dc1"]
  group "consul" {
    count = 1
    task "consul" {
      driver = "raw_exec"
            
      config {
        command = "consul"
        args    = ["agent", "-client", "0.0.0.0", "-dev"]
      }
      artifact {
        source = "https://releases.hashicorp.com/consul/1.7.3/consul_1.7.3_linux_amd64.zip"
      }
    }
  }
}
```

### Setting up Fabio in Nomad

We run Fabio in a similar way as we did for Consul - using a binary. Job below.

```
job "fabio" {
  datacenters = ["dc1"]
  group "fabio" {
    count = 1
    task "fabio" {
      driver = "raw_exec"
      env {
        proxy_addr = ":80;:8300"
      }
      config {
        command = "fabio"
        args    = ["-proxy.strategy=rr"]
      }
      artifact {
        source      = "https://github.com/fabiolb/fabio/releases/download/v1.5.13/fabio-1.5.13-go1.13.4-linux_386"
        destination = "local/fabio"
        mode        = "file"
      }
    }
  }
}
```

### Using the web UI's

If everything worked as expected you should be able to see the following ui's:

* Nomad: localhost:4646
* Consul: localhost:8500
* Fabio: localhost:9998

### Setting up Filestash

We will need 1 task inside 1 job and 1 task group. The 1 task is going to be `filestash`. We need to make sure we persist filestash `/app/data` localy using volumes.

```
job "filestash" {
  datacenters = ["dc1"]
  type = "service"
  group "filestash" {
    count = 1
    task "filestash" {
      driver = "docker"
      config {
        image = "machines/filestash"
        volumes = [
          "/app_storage/filestash/data:/app/data"
        ]
        port_map {
          http = 8334
        }
      }
      resources {
        network {
          mbits = 10
          port "http" {}
        }
      }
      service {
        name = "filestash"
        tags = [
          "urlprefix-cloud.yourdomain.com/",
        ]
        port = "http"
        check {
          name     = "alive"
          type     = "http"
          path     = "/"
          interval = "10s"
          timeout  = "2s"
        }
      }
    }
  }
}
```

### DNS subdomain

If you are using a dns, you will want to setup a subdomain. Here we use "cloud" as subdomain.

### Setting up Fabio for load balancing

If you have Consul and Fabio correctly installed the service stanza in the above nomad job will register the `cloud` service on the `urlprefix-`. Note since we are using a subdomain there is nothing else you need to do. All filestash urls will go to `cloud.`.

Here is the stanza in the `filestash` job that registers your service to Consul and Fabio.

```
service {
	name = "filestash"
    tags = [
        "urlprefix-cloud.yourdomain.com/",
    ]
    port = "http"
    check {
		name     = "alive"
        type     = "http"
        path     = "/"
        interval = "10s"
        timeout  = "2s"
     }
}
```

###  Volumes 

In the above nomad job we have configured a volume for Filestash `"/app_storage/filestash/data:/app/data"`. The purpose of this volume is to persist the Filestash container data to the host OS.

### Spinning up Filestash with nomad using the cli

If you prefer using the cli, here is how you spin up a nomad job from the command line.

`nomad job run filestash.nomad`

If we do `nomad status` we should be able to see that our Filestash container is running. If we go to `cloud.yourdomain.com` we should see Filestash admin page (if we log for the first time).
