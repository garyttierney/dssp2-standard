# Standard Defensec SELinux Security Policy Version 2

## Introduction

Standard DSSP (Version2) is an example policy configuration that is built on top of Minimal DSSP (Version 2).

## Leverages Common Intermediate Language

Common Intermediate Language is a language that is native to SELinux and that implements functionality inspired by popular features of Tresys Reference policy without the need for a pre-processor.

The source policy oriented nature of CIL provides enhanced accessibility and modularity. Text-based configuration makes it easier to resolve dependencies and to profile.

New language features allows authors to focus on creativity and productivity. Clear and simple syntax makes it easy to parse and generate security policy.

## Requirements

DSSP requires `semodule` or `secilc` version 2.4 and higher.

SELinux should be enabled in the Linux kernel, your file systems should support `security extended attributes` and this support should be enabled in the Linux kernel.

## Installation

    git clone --recurse https://github.com/defensec/dssp2-standard
    cd dssp2-standard
    make install-semodule
	cat > /etc/selinux/config <<EOF
    SELINUX=enforcing
    SELINUXTYPE=dssp2-standard
    EOF
    echo "-F" > /.autorelabel
    reboot

## Known issues

Various `systemd` socket units and `systemd-tmpfiles` configuration snippets may refer to `/var/run` instead of `/run` and this causes them to create content with the wrong security context.

Fedora:

	cp /usr/lib/tmpfiles.d/pam.conf /etc/tmpfiles.d/ && \
		sed -i 's/\/var\/run/\/run/' /etc/tmpfiles.d/pam.conf

	cp /usr/lib/tmpfiles.d/libselinux.conf /etc/tmpfiles.d/ && \
		sed -i 's/\/var\/run/\/run/' /etc/tmpfiles.d/libselinux.conf

Debian:

	cp /usr/lib/tmpfiles.d/sshd.conf /etc/tmpfiles.d/
		sed -i 's/\/var\/run/\/run/' /etc/tmpfiles.d/sshd.conf

	cp /lib/systemd/system/dbus.socket /etc/systemd/system/
		sed -i 's/\/var\/run/\/run/' /etc/systemd/system/dbus.socket

	cp /lib/systemd/system/avahi-daemon.socket /etc/systemd/system/
		sed -i 's/\/var\/run/\/run/' /etc/systemd/system/avahi-daemon.socket

## Resources

* [Common Intermediate Language](https://github.com/SELinuxProject/selinux/blob/master/secilc/docs/README.md) Learn to speak CIL
