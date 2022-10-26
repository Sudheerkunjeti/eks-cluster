# eks-cluster

Steps to install the latest version of EKSCTL, KUBECTL and AWS-IAM-Authenicator.

**To install or update eksctl on Linux**

1. Download and extract the latest release of eksctl with the following command.

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

2. Move the extracted binary to /usr/local/bin.

sudo mv /tmp/eksctl /usr/local/bin

3. Test that your installation was successful with the following command.

eksctl version


Note
The GitTag version should be at least 0.115.0. If not, check your terminal output for any installation or upgrade errors, or replace the address in step 1 with https://github.com/weaveworks/eksctl/releases/download/v0.115.0/eksctl_Linux_amd64.tar.gz and complete steps 1-3 again.


**Install kubectl binary with curl on Linux**

1. Download the latest release with the command:

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

Note:
To download a specific version, replace the $(curl -L -s https://dl.k8s.io/release/stable.txt) portion of the command with the specific version.

For example, to download version v1.25.0 on Linux, type:

curl -LO https://dl.k8s.io/release/v1.25.0/bin/linux/amd64/kubectl

2. Validate the binary (optional)

Download the kubectl checksum file:

curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
Validate the kubectl binary against the checksum file:

echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
If valid, the output is:

kubectl: OK
If the check fails, sha256 exits with nonzero status and prints output similar to:

kubectl: FAILED
sha256sum: WARNING: 1 computed checksum did NOT match
Note: Download the same version of the binary and checksum.

3. Install kubectl

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
Note:
If you do not have root access on the target system, you can still install kubectl to the ~/.local/bin directory:

chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
# and then append (or prepend) ~/.local/bin to $PATH

4. Test to ensure the version you installed is up-to-date:

kubectl version --client
Or use this for detailed view of version:

kubectl version --client --output=yaml    

**Installing aws-iam-authenticator**

To install aws-iam-authenticator on Linux

1. Download the aws-iam-authenticator binary from GitHub for your hardware platform. The first command downloads the amd64 release. The second command downloads the arm64 release.

curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.5.9/aws-iam-authenticator_0.5.9_linux_amd64

2. (Optional) Verify the downloaded binary with the SHA-256 checksum for the file.

Download the SHA-256 checksum file.

curl -Lo aws-iam-authenticator.txt https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.5.9/authenticator_0.5.9_checksums.txt
View the checksum for the authenticator binary that you downloaded. The first command returns the amd64 checksum. The second command returns the arm64 checksum.

awk '/aws-iam-authenticator_0.5.9_linux_amd64/ {print $1}' aws-iam-authenticator.txt
awk '/aws-iam-authenticator_0.5.9_linux_arm64/ {print $1}' aws-iam-authenticator.txt
The example output is as follows.

b192431c22d720c38adbf53b016c33ab17105944ee73b25f485aa52c9e9297a7
Determine the SHA-256 checksum for your downloaded binary.

openssl sha1 -sha256 aws-iam-authenticator
The example output is as follows.

SHA256(aws-iam-authenticator)= b192431c22d720c38adbf53b016c33ab17105944ee73b25f485aa52c9e9297a7
The returned output should match the output returned in the previous step.

3. Apply execute permissions to the binary.

chmod +x ./aws-iam-authenticator

4. Copy the binary to a folder in your $PATH. We recommend creating a $HOME/bin/aws-iam-authenticator and ensuring that $HOME/bin comes first in your $PATH.

mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$PATH:$HOME/bin

5. Add $HOME/bin to your PATH environment variable.

echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc

6. Test that the aws-iam-authenticator binary works.

aws-iam-authenticator help

