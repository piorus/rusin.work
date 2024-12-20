---
title: 'Certified Kubernetes Application Developer (CKAD) Tips'
draft: false
slug: 'ckad-tips'
date: '2024-07-07'
---

Today, I attempted the Certified Kubernetes Application Developer (CKAD) certification. Below is a set of tips and insights that allowed me to pass it smoothly.

## Sources

I relied on [Mumshad Mannambeth's course](https://www.udemy.com/course/certified-kubernetes-application-developer) on Udemy. The course cost around 50 PLN and is worth every penny.  
For each discussed topic, the course provides an independent Kubernetes environment along with a series of tasks to complete.  
Although the environment does not fully replicate the one in the exam, it helps with mastering YAML generation/editing and `kubectl` commands.

To complement this, I used [Killerconda](https://killercoda.com/killer-shell-ckad) and their environment, which covers several key topics relevant to the certification.

The day before the scheduled exam, I took a practice test from [killer.sh](https://killercoda.com/killer-shell-ckad). I scored slightly above 66% within 2 hours. For the rest of the day, I filled in my gaps and worked on the tasks I didn’t manage to complete or didn’t know how to solve during the test.

## Commands

During the certification, you need to connect to a separate host for each task via ssh. The command you need to execute along with the host is embedded in each task. For example:

```bash
ssh ckad0123
```

Almost every task must be completed within a given namespace. It’s helpful to open a separate terminal window and prepare the command to switch namespaces:

```bash
kubectl config set-context --current --namespace
```

You can copy and paste this command directly onto the host where you will perform the task, for example:

```bash
ssh ckad0123
controlplane@ckad0123 > kubectl config set-context --current --namespace elephant
```

Additionally, it’s worth remembering declarative pod and deployment creation:

```bash
k run poddy --image nginx
```

This will create and start a pod named poddy using the nginx image.

Sometimes, for more complex tasks that cannot be solved purely via the CLI, it’s good to remember dry-run mode and generating YAML files using kubectl:

```bash
k run poddy --image nginx -o yaml --dry-run=client > pod.yaml
vi pod.yaml
```

Similarly, for deployments:

```bash
k create deploy nginx-deployment --image nginx:latest --replicas 3
```

Helpful commands include:

```bash
k label
k set image
k replace --force -f some.yaml
```