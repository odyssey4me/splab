# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# To ensure a consistent test, ensure that we set
# a minimal Vagrant version.
Vagrant.require_version ">= 1.7.0"

Vagrant.configure(2) do |config|

  config.vm.define "ubuntu1604" do |xenial|
    xenial.vm.box = "ubuntu/xenial64"
    xenial.vm.provider "virtualbox" do |v|

      v.name = "SPLAB_#{Time.now.getutc.to_i}"
      v.memory = 2048
      v.cpus = 2

      image_path = "#{ENV["HOME"]}/VirtualBox VMs/#{v.name}"
      image_name = 'ubuntu-xenial-16.04-cloudimg'

      # We clone the image to a resizable format
      v.customize [
        "clonehd", "#{image_path}/#{image_name}.vmdk",
                   "#{image_path}/#{image_name}.vdi",
        "--format", "VDI"
      ]

      # Then resize it to 25 GB
      v.customize [
        "modifymedium", "disk",
        "#{image_path}/#{image_name}.vdi",
        "--resize", 25 * 1024
      ]

      # Then attach it as the primary disk
      v.customize [
        "storageattach", :id,
        "--storagectl", "SCSI",
        "--port", "0",
        "--device", "0",
        "--type", "hdd",
        "--nonrotational", "on",
        "--medium", "#{image_path}/#{image_name}.vdi"
      ]

      # Then remove the original disk
      v.customize [
        "closemedium", "disk",
        "#{image_path}/#{image_name}.vmdk",
        "--delete"
      ]

      # Now we can execute the build
      config.vm.provision "shell",
        privileged: false,
        inline: <<-SHELL
          cd /vagrant/tests
          ./run_tests.sh
        SHELL

    end
  end

end
