# rpi-ansible-podman-systemd

A small Ansible role for running containers with Podman/systemd on a Raspberry Pi.
It takes care of:
* Installing Podman and its dependencies for running rootless.
* Managing a systemd unit service file for each container.
* Creating the required containers volumes (for persistency).

# Role requirements

* Ansible version 2.9+
* Raspberry Pi with Raspbian buster or Fedora 32

# Dependencies

None.

# Role variables

`podman_containers`: array of container definitions.

# Example playbook

    - hosts: myraspberrypi
      tasks:
        - name: Run Mosquitto MQTT container as a service
          vars:
            podman_containers:
              - name: mqtt
                image: eclipse-mosquitto:1.6.12
                ports:
                  - 1883:1883
                  - 9001:9001
                user: pi
                volumes:
                  - name: mosquitto-data
                    mountPath: /mosquitto/data
                  - name: mosquitto-log
                    mountPath: /mosquitto/log
                  - hostPath: /home/pi/mqtt/mosquito.conf
                    mountPath: /mosquitto/config/mosquitto.conf
          import_role:
            name: rpi-ansible-podman-systemd
