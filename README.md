# numpy-2.3.5-cp313-cp313-linux_armv7l
Numpy 2.3.5 wheel for python 3.13 on linux running on armv7

# Building Numpy wheel on armv7
1. Download numpy release of desired version from e.g., https://pypi.org/project/numpy/#files
2. Create swap memory on the red pitaya
	```
	# Create a 1 Gib file
		# if=/dev/zero (input file is /dev/zero which provides as many null characters as requested)
		# of=/mnt/1GiB.swap (output file is called /mnt/1GiB.swap)
		# bs=1024 (block size of operation)
		# count=1048576 (size of the file in kiB)
	sudo dd if=/dev/zero of=/mnt/1GiB.swap bs=1024 count=1048576
	# Set the right permissions
	sudo chmod 600 /mnt/1GiB.swap
	# Configure it to be swap
	sudo mkswap /mnt/1GiB.swap
	# Enable the use of the swap file
	sudo swapon /mnt/1GiB.swap
	# Add it to fstab so it is usable on boot
	echo '/mnt/1GiB.swap swap swap defaults 0 0' | sudo tee -a /etc/fstab
	```
3. Create uv environment with requisite python version
	`uv init build_temp --python 3.13`
4. Install meson-python and build
	`uv add build meson-python`
5. Activate the environment
	`source .venv/bin/activate`
6. Naviagate to root directory of wherever the unzipped numpy source is
7. Run the build command. It will likely take ~1hr to build.
	`python -m build --wheel`
8. The `.whl` file can be found in the newly created `dist/` directory. Copy off the remote.
9. Upload to git hub.

# How to add the newly built .whl as an source only on armv7 architecture
1. Get the raw download link, not just the plain url of the file.
2. Add the following to the pyproject.toml
```
# Ensure "numpy==2.3.5" is set in the dependancy

# The following adds the url as a source for numpy only when the machine architecture is armv7l
[tool.uv.sources]
numpy = { url = "raw_download_link.whl", marker="platform_machine == 'armv7l'" }

```
