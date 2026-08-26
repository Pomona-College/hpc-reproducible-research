# Setup Instructions

## Before the Workshop

To get the most out of this workshop, please complete these setup steps before it begins.

## Requirements

### Local Machine

You need:

- **Terminal/Shell access**: Unix command line (Mac, Linux, or Windows with WSL2)
- **Text editor**: VS Code, Sublime Text, nano, vim, or similar
- **git**: Version control (https://git-scm.com/downloads)
- **conda**: Environment management (https://docs.conda.io/projects/miniconda/en/latest/)
- **Make**: Workflow automation (usually pre-installed on Mac/Linux; see below for Windows)

### HPC Access

- **Access to Sagehen HPC** (sagehen.hpc.pomona.edu) or another SLURM-based HPC cluster
- **SSH key pair** configured for cluster access
- Familiarity with `ssh` and basic command-line navigation

## Installation Guide

### 1. Install git

**Mac:**
```bash
# Install Xcode command line tools (includes git)
xcode-select --install
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt-get install git
```

**Windows:**

- Download from https://git-scm.com/download/win
- Use default options during installation
- This gives you Git Bash (a terminal with Unix commands)

**Verify:**
```bash
git --version
# Should show: git version X.X.X
```

### 2. Install conda

Download Miniconda from: https://docs.conda.io/projects/miniconda/en/latest/

**Mac (Intel):**

```bash
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh
bash Miniconda3-latest-MacOSX-x86_64.sh
```

**Mac (Apple Silicon/M1/M2):**
```bash
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
bash Miniconda3-latest-MacOSX-arm64.sh
```

**Linux:**

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```

**Windows:**

- Download installer from https://docs.conda.io/projects/miniconda/en/latest/
- Run the .exe file
- Choose "Install for current user" (do NOT install system-wide)

**Verify:**

```bash
conda --version
# Should show: conda X.X.X
```

### 3. Install Make

**Mac:**

```bash
# Included with Xcode command line tools
xcode-select --install
```

**Linux (Ubuntu/Debian):**

```bash
sudo apt-get install build-essential
```

**Windows (WSL2):**

```bash
# Within WSL2 Ubuntu environment
sudo apt-get install build-essential
```

**Verify:**

```bash
make --version
# Should show: GNU Make X.X.X
```

### 4. Set Up HPC Access

Test your connection to Sagehen:

```bash
ssh <myusername>@sagehen.hpc.pomona.edu
```

You should see a welcome message and command prompt. If you get permission denied:

1. Verify your username is correct
2. Check that your SSH key is set up (contact HPC support if needed)
3. Check your firewall/VPN if off-campus

From Sagehen, verify the module system works:

```bash
module list
module load miniconda3
conda --version
```

## Creating Your First Environment

Test that you can create and use a conda environment:

```bash
# Create an environment
conda create -n test-env python=3.11 numpy pandas

# Activate it
conda activate test-env

# Verify it works
python -c "import numpy, pandas; print('Success!')"

# Deactivate
conda deactivate
```

## Verify Your Setup

Run this script to check everything is working:

```bash
#!/bin/bash

echo "Checking prerequisites..."

# Check git
if command -v git &> /dev/null; then
    echo "✓ git: $(git --version)"
else
    echo "✗ git not found"
fi

# Check conda
if command -v conda &> /dev/null; then
    echo "✓ conda: $(conda --version)"
else
    echo "✗ conda not found"
fi

# Check make
if command -v make &> /dev/null; then
    echo "✓ make: $(make --version | head -1)"
else
    echo "✗ make not found"
fi

# Check HPC access
if ping -c 1 sagehen.hpc.pomona.edu &> /dev/null; then
    echo "✓ Network access to sagehen.hpc.pomona.edu"
else
    echo "✗ Cannot reach sagehen.hpc.pomona.edu (may be expected if off-campus)"
fi

echo ""
echo "All prerequisites ready!"
```

Save as `check_setup.sh`, run with `bash check_setup.sh`

## Troubleshooting

### conda: command not found

Your shell doesn't know about conda. After installing, you need to restart your terminal or run:

```bash
# Reload your shell configuration
source ~/.bashrc
# or on Mac:
source ~/.zprofile
```

### git: command not found (Mac)

Install Xcode command line tools:

```bash
xcode-select --install
```

### Permission denied (publickey) for sagehen

Contact HPC support (its-hpc@pomona.edu) to verify:

- Your account is active
- Your SSH key is installed
- You have access to Sagehen

### Command not found: make

Install build-essential (Linux) or Xcode tools (Mac):

**Linux:**

```bash
sudo apt-get update
sudo apt-get install build-essential
```

**Mac:**

```bash
xcode-select --install
```

## Getting Help

- **HPC issues:** its-hpc@pomona.edu
- **Workshop questions:** Reach out to the instructor
- **Community:** Join [The Carpentries community](https://carpentries.org/about-us/community/) for broader questions

## Next Steps

Once you've completed setup:

1. Clone the workshop materials (instructions will be provided)
2. Review the [Learner Profiles](learner-profiles.md) to see if you fit a described profile
3. Skim the [Reference](reference.md) for an overview of key concepts
4. You're ready to begin Episode 1!

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
