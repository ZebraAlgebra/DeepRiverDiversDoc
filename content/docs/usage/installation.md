---
weight: 1
title: "Installation"
---

# Installation

## Step 1. Clone Repository and cd

{{< tabs "git clone" >}}
{{% tab "HTTPS" %}}
```bash
git clone https://github.com/yindrew/DeepRiverDivers.git
cd DeepRiverDivers
```
{{% /tab %}}
{{% tab "SSH" %}}
```bash
git clone git@github.com:yindrew/DeepRiverDivers.git
cd DeepRiverDivers
```
{{% /tab %}}
{{% tab "GitHub CLI" %}}
```bash
gh repo clone yindrew/DeepRiverDivers
cd DeepRiverDivers
```
{{% /tab %}}
{{< /tabs >}}

## Step 2. (Optional) Intialize Virtual Environment

{{< tabs "virtual env" >}}
{{% tab "Native Python venv" %}}
```bash
python -m venv .venv
source .venv/activate.fish # depends on your shell and OS
```
{{% /tab %}}
{{% tab "Using uv" %}}
```bash
uv venv .venv # replace .venv by other names for virtual env names
source .venv/activate.fish # depends on your shell and OS
```
{{% /tab %}}
{{< /tabs >}}

## Step 3. Install python packages

{{< tabs "install" >}}
{{% tab "Native pip" %}}
```bash
pip3 install -r requirement.txt
```
{{% /tab %}}
{{% tab "Using uv" %}}
```bash
uv pip install -r requirement.txt
```
{{% /tab %}}
{{< /tabs >}}
