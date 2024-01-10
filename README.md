# Buildpacks

A buildpack is a set of executables that inspects your app source code and creates a plan to build and run your application.

Typical buildpacks consist of at least three files:

- **buildpack.toml**: provides metadata about your buildpack
- **bin/detect**: determines whether the buildpack should be applied
- **bin/build**: executes build logic.

## Roadmap to Make a Working Buildpack

### Step 1: Get the Corresponding Repository Ready
Make sure the repository containing your source code is prepared.

### Step 2: Update the Placeholders Required
Update any necessary placeholders in your buildpack files.

### Step 3: Build the Buildpack (.cnb file)

Execute the build logic to generate the .cnb file as output.

#### Command:
```bash
./scripts/package.sh --version <version>
```

Execute the build logic to generate the .cnb file as output.

### Step 4: Push the .cnb to ECR (Using skopeo)

Use skopeo to push the generated .cnb file to the Elastic Container Registry (ECR).

#### Command:
```bash
skopeo copy "oci-archive:<path/uri to .cmb file>" "docker://<container registry link>"
```

### Step 5.a: Register the Essential Buildpacks on the CNB Registry (As of Now)
Register the required essential buildpacks on the CNB registry.

### Command:
```bash
pack buildpack register <Uri/Path of the cnb file>
```
### Step 5.b: Use Paketo for Non-Essentials (As of Now)
Utilize Paketo for non-essential buildpacks.

### Step 6: Enlist All Buildpacks into the toml File of the Main Buildpack
List all the buildpacks in the toml file of the main buildpack.

### Step 7: Repeat Steps 3 to 5.a
Repeat the process from building the buildpack to registering essential buildpacks.

### Step 8: Update the Buildpack into the builder.toml
Update the buildpack information in the builder.toml file.

### Step 9: Create the Builder on Local
Create the builder locally with the updated buildpack information.
### Command:
```bash
pack builder create name:tag --config ./builder.toml
```
### Step 10: Test the Builder on Corresponding Source Code
Test the builder with the source code to ensure everything works as expected.
### Command:
```bash
pack build <imagename> --builder <builder name and tag > --path <location of source file>
```
### Step 11: If Everything Works Fine, Push the Builder to ECR
If the testing is successful, push the builder to the Elastic Container Registry (ECR).
### Command:
```bash
docker tag builder "<uri>"
docker push "URI"
```

For more detailed information on buildpacks, refer to the [Buildpacks Documentation](https://buildpacks.io/docs/).
