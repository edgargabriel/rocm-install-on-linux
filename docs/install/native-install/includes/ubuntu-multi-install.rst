.. _ubuntu-multi-install:

1. Set up your package signing key following the steps in :ref:`ubuntu-package-key`.

2. Register the kernel-mode driver.

   Add the AMDGPU repository for the driver.

   .. datatemplate:nodata::

       .. tab-set::
           {% for (os_version, os_release) in config.html_context['ubuntu_version_numbers'] %}
           .. tab-item:: Ubuntu {{ os_version }}
               :sync: ubuntu-{{ os_version}}

               .. code-block:: bash
                   :substitutions:

                   for ver in |rocm_multi_versions|; do
                   echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/rocm.gpg] https://repo.radeon.com/amdgpu/$ver/ubuntu {{ os_release }} main" \
                       | sudo tee /etc/apt/sources.list.d/amdgpu.list
                   done
                   sudo apt update
           {% endfor %}

.. _ubuntu-multi-register-rocm:

3. Register ROCm packages.

   Add the ROCm repository.

   .. datatemplate:nodata::

      .. tab-set::
          {% for (os_version, os_release) in config.html_context['ubuntu_version_numbers'] %}
          .. tab-item:: Ubuntu {{ os_version }}
              :sync: ubuntu-{{ os_version}}
  
              .. code-block:: bash
                  :substitutions:
  
                  for ver in |rocm_multi_versions|; do
                  echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/rocm.gpg] https://repo.radeon.com/rocm/apt/$ver {{ os_release }} main" \
                      | sudo tee --append /etc/apt/sources.list.d/rocm.list
                  done
                  echo -e 'Package: *\nPin: release o=repo.radeon.com\nPin-Priority: 600' \
                      | sudo tee /etc/apt/preferences.d/rocm-pin-600
                  sudo apt update
          {% endfor %}

4. Install ROCm.

   a. Install the kernel driver.

      .. code-block:: bash

         sudo apt install amdgpu-dkms
         sudo reboot

   b. Install the registered ROCm packages.

      .. code-block:: bash
         :substitutions:

         for ver in |rocm_multi_versions|; do
             sudo apt install rocm$ver
         done

5. Complete the :doc:`../post-install`.

.. tip::

   For a single-version installation of the latest ROCm version on Ubuntu,
   use the steps in :ref:`ubuntu-register-repo` and :ref:`ubuntu-install`.

