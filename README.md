# SNUPI MLXP Helper Scripts

Helper scripts installed into managed MLXP reservation images at:

```text
~/scripts
```

The scripts are intended for already-running reservation pods. They do not
replace controller-managed registry pull credentials.

## Scripts

- `snupi-docker-login`: persist Docker registry auth under `~/work/.docker`
- `snupi-git-setup`: persist GitHub CLI auth under `~/work/.config/gh`
  and global Git config under `~/work/.gitconfig`

Both helpers prefer DDN-backed `~/work` storage so credentials survive pod
recreation. Treat these directories as sensitive.

## Docker Hub Build/Push Auth

Run once inside a managed reservation pod:

```bash
snupi-docker-login
```

The helper writes Docker auth to:

```text
~/work/.docker/config.json
```

It also appends this to `~/work/.bashrc`:

```bash
export DOCKER_CONFIG="$HOME/work/.docker"
```

Use this for build and push from inside a reservation. Pulling the reservation
image itself still uses controller-managed `registryProfiles`.

## GitHub Auth

The default mode uses GitHub CLI with HTTPS Git credentials:

```bash
snupi-git-setup
```

The helper stores GitHub CLI auth under:

```text
~/work/.config/gh
```

It also stores global Git config under:

```text
~/work/.gitconfig
```

For SSH-based Git instead:

```bash
snupi-git-setup --protocol ssh
```

SSH mode creates a key under `~/work/.ssh` and attempts to upload the public key
with `gh ssh-key add`. If upload fails, the script prints the public key so the
member can add it manually in GitHub.
