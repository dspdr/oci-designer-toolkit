# OCI Kinetic Infrastructure Toolkit

OCI Kinetic Infrastructure Toolkit (OKIT) is a a series of tools that allow the user to Query, Capture, Visualise and
Generate DevOps tooling scripts. Using the toolkit the DevOps user will be able to capture existing OCI environments,
through simple command line or web based interface, and then convert them to standard DevOps languages (see below). 
The web based drawing toll can also be used to generate simple architectural representations of customer systems that 
can quickly be turned into code and ultimately built in the OCI environment.

## Table of Contents

1. [Installation](#installation)
2. [Usage](#usage)
    1. [Currently Implemented Artifacts](#currently-implemented-artifacts)
    2. [Prerequisites](#prerequisites)
    3. [Web Interface](#web-interface)
    4. [Command Line](#command-line)
3. [Development](#development)
4. [Contributing](#contributing)
5. [Examples](#examples)

## Installation
Although OKIT can simply be downloaded and the command line executed it is recommended that it be executed within a
docker container that can be built using the Dockerfile within this repository. This will guarantee that all the required 
python modules are installed and in addition provide a simple flask server that will present the web based application on
[http://localhost:8080/okit/designer](http://localhost:8080/okit/designer).

Therefore these installation instructions will describe the docker based implementation.

```bash
anhopki-mac:tmp anhopki$ git clone git@orahub.oraclecorp.com:andrew.hopkinson/okit.oci.web.designer.git

Cloning into 'okit.oci.web.designer'...
===============================================================================
            Oracle USA Inc. - Unauthorized Access Strictly Prohibited
-------------------------------------------------------------------------------
   WARNING: This is a restricted access server. If you do not have explicit
            permission to access this server, please disconnect immediately.
            Unauthorized access to this system is considered gross misconduct
            and may result in disciplinary action, including revocation of
            access privileges, immediate termination of employment.
===============================================================================
remote: Enumerating objects: 448, done.
remote: Counting objects: 100% (448/448), done.
remote: Compressing objects: 100% (208/208), done.
remote: Total 1455 (delta 287), reused 376 (delta 229)00 KiB/s
Receiving objects: 100% (1455/1455), 604.46 KiB | 798.00 KiB/s, done.
Resolving deltas: 100% (859/859), done.

anhopki-mac:tmp anhopki$ cd okit.oci.web.designer/docker/
anhopki-mac:docker anhopki$ lh

total 56
0 drwxr-xr-x  10 anhopki  staff   320B 15 Aug 10:08 .
0 drwxr-xr-x   9 anhopki  staff   288B 15 Aug 10:08 ..
8 -rwxr-xr-x   1 anhopki  staff   932B 15 Aug 10:08 build-docker-image.sh
8 -rwxr-xr-x   1 anhopki  staff   238B 15 Aug 10:08 connect-flask-bash-shell.sh
8 -rwxr-xr-x   1 anhopki  staff   241B 15 Aug 10:08 connect-gunicorn-bash-shell.sh
0 drwxr-xr-x   3 anhopki  staff    96B 15 Aug 10:08 docker
8 -rwxr-xr-x   1 anhopki  staff   1.6K 15 Aug 10:08 docker-env.sh
8 -rwxr-xr-x   1 anhopki  staff   512B 15 Aug 10:08 start-bash-shell.sh
8 -rwxr-xr-x   1 anhopki  staff   621B 15 Aug 10:08 start-flask.sh
8 -rwxr-xr-x   1 anhopki  staff   654B 15 Aug 10:08 start-gunicorn.sh

anhopki-mac:docker anhopki$ ./build-docker-image.sh
```

## Usage
### Currently Implemented Artifacts
In the present release the following OCI artifacts have been implemented. The information captured in the properties may 
only be the minimum to create the artifacts but will be extended in the future.

- Virtual Cloud Network 
- Internet Gateway
- Route Table
- Security List
- Subnet
- Instance
- Load Balancer

### Prerequisites
Before executing any of the docker container scripts we OKIT requires that a OCI connection configuration file be created.
This file will contain the following:

```properties
[DEFAULT]
user=ocid1.user.oc1..aaaaaaaak6z......
fingerprint=3b:7e:37:ec:a0:86:1....
key_file=/root/.oci/oci_api_key.pem
tenancy=ocid1.tenancy.oc1..aaaaaaaawpqblfem........
region=us-phoenix-1

```
You will then need to create the following environment variable that points to the directory containing the config file.

```bash
export OCI_CONFIG_DIR=~/.oci/oci.config
```

### Web Interface
To use the Web Application you will first need to start the docker container and run either flask or gunicorn which can 
be achieved by running either of the following scripts, that can be found in the docker sub-directory.

```bash
anhopki-mac:docker anhopki$ ./start-flask.sh

DOCKERIMAGE = development/orahub/andrew.hopkinson/okit.oci.web.designer
/okit
HOSTNAME=start-flask
TERM=xterm
ANSIBLE_INVENTORY=/okit/ansible/config/ansible_hosts
LC_ALL=en_GB.UTF-8
FLASK_APP=okitweb
PYTHONIOENCODING=utf8
http_proxy=
ftp_proxy=
ANSIBLE_LIBRARY=:
PATH=/root/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/okit
LANG=en_GB.UTF-8
FLASK_DEBUG=development
https_proxy=
SHLVL=1
HOME=/root
LANGUAGE=en_GB:en
no_proxy=*
ANSIBLE_CONFIG_DIR=/okit/ansible/config
PYTHONPATH=:/okit/visualiser:/okit/okitweb:/okit
ANSIBLE_CONFIG=/okit/ansible/config/ansible.cfg
_=/usr/bin/env
 * Serving Flask app "okitweb" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on http://0.0.0.0:8080/ (Press CTRL+C to quit)
 * Restarting with stat
```

```bash
anhopki-mac:docker anhopki$ ./start-gunicorn.sh

DOCKERIMAGE = development/orahub/andrew.hopkinson/okit.oci.web.designer
/okit/okitweb
HOSTNAME=start-gunicorn
TERM=xterm
ANSIBLE_INVENTORY=/okit/ansible/config/ansible_hosts
LC_ALL=en_GB.UTF-8
FLASK_APP=okitweb
PYTHONIOENCODING=utf8
http_proxy=
ftp_proxy=
ANSIBLE_LIBRARY=:
PATH=/root/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/okit/okitweb
LANG=en_GB.UTF-8
FLASK_DEBUG=development
https_proxy=
SHLVL=1
HOME=/root
LANGUAGE=en_GB:en
no_proxy=*
ANSIBLE_CONFIG_DIR=/okit/ansible/config
PYTHONPATH=:/okit/visualiser:/okit/okitweb:/okit
ANSIBLE_CONFIG=/okit/ansible/config/ansible.cfg
_=/usr/bin/env
[2019-08-15 14:32:05 +0000] [7] [INFO] Starting gunicorn 19.9.0
[2019-08-15 14:32:05 +0000] [7] [INFO] Listening at: http://0.0.0.0:8080 (7)
[2019-08-15 14:32:05 +0000] [7] [INFO] Using worker: sync
[2019-08-15 14:32:05 +0000] [10] [INFO] Booting worker with pid: 10
[2019-08-15 14:32:05 +0000] [11] [INFO] Booting worker with pid: 11
```
#### Designer
The Designer consists of 3 main areas.
1. Palette
2. Canvas
3. Properties Sheet

The palette should all currently available assets and they can be dragged from the palette on the canvas below. During the 
drag process the icon will indicate if the palette icon can be dropped on the underlying component. If appropriate a "+" 
sign will be displayed. Once the icon has been dropped the appropriate properties sheet will be displayed for the new asset.

If assets (e.g. subnet) are dropped within a containing asset (e.g. virtual cloud network) then it will be associated with the 
containing asset. Alternatively assets may be linked using connectors which can be dragged from the asset to associate
(e.g. route table) to the associated asset (e.g. subnet). Again the drop is only appropriate on certain components and 
the web front end will restrict this.

A simple diagram representing a virtual Cloud Network with associated Internet Gateway, Route Table, Security List and 
Instances fronted by a Load Balancer can be seen below.

![OKIT Designer Canvas](documentation/images/okit_web_interface.png?raw=true "OKIT Designer Canvas")

The hamburger menu in the top left will display a slide out menu with all available actions (described below).

##### Palette
- <img src="okitweb/static/okit/palette/Virtual_Cloud_Network.svg?sanitize=true" width="30" height="30"/> Virtual Cloud Network 
- <img src="okitweb/static/okit/palette/Internet_Gateway.svg?sanitize=true" width="30" height="30"/> Internet Gateway
- <img src="okitweb/static/okit/palette/Route_Table.svg.svg?sanitize=true" width="30" height="30"/> Route Table
- <img src="okitweb/static/okit/palette/Security_List.svg.svg?sanitize=true" width="30" height="30"/> Security List
- <img src="okitweb/static/okit/palette/Subnet.svg.svg?sanitize=true" width="30" height="30"/> Subnet
- <img src="okitweb/static/okit/palette/Instance.svg.svg?sanitize=true" width="30" height="30"/> Instance
- <img src="okitweb/static/okit/palette/Load_Balancer.svg.svg?sanitize=true" width="30" height="30"/> Load Balancer

##### Menu 
![OKIT Web Interface Menu](documentation/images/okit_menu.png?raw=true "OKIT Web Interface Menu")

- File
    - New
    - Load
    - Save
- Canvas
    - Redraw
- Export
    - SVG
    - Resource Manager
    - Orca
- Query
    - OCI
- Generate
    - Terraform
    - Ansible

###### File/New
Creates a new clear canvas.
###### File/Load
Allows the user to select a previously saved or command line generated json file.
###### File/Save
Saves the current diagram as a json representation.
###### Canvas/Redraw
Redraws the existing canvas this will have the effect of grouping similar assets.
###### Export/SVG
Will export the current diagram as an SVG object that can be distributed.
###### Export/Resource Manager
***(WIP)*** Will generate Terraform code and export the resulting zip file into the OCI Resource Manager.
###### Export/Orca
***(WIP)*** Convert the OKIT JSON format to Orca input format.
##### Query/OCI
Opens Query pages and populates the Compartment list. Once the user has chosen the compartment and added a virtual cloud network 
filter submitting will query OCI and draw the returning assets on the new designer canvas.
##### Generate/Terraform
Generate a set of Terraform that can be used to build the designed OCI infrastructure currently loaded and return as a zip file.
##### Generate/Ansible
Generate a set of Ansible that can be used to build the designed OCI infrastructure currently loaded  and return as a zip file.

### Command Line
To use the Command Line you will first need to start the docker container using the following script:
```bash
anhopki-mac:docker anhopki$ ./start-bash-shell.sh
DOCKERIMAGE = development/orahub/andrew.hopkinson/okit.oci.web.designer
[root@start-bash-shell workspace]#
```
Alternatively if you are running the Webb Application you can connect to the existing docker contain using either:

- connect-flask-bash-shell.sh
- connect-gunicorn-bash-shell.sh

The command line interface consists of a number of python programs as follows:
#### Capture
***(WIP)*** The capture python will connect to the specified OCI instance (defaults to config in users home directory) and 
queries OCI based on the specified filters. The resulting data is then written to the specified OKIT json file (okit.json)
and can be subsequently imported into the web based interface or used to generate the DevOps files (see [Generate](#generate)).
#### Validate
***(WIP)*** The validate python will read the specified OKIT json file (okit.json) and check if the references are consistent.
#### Generate
The generate python process takes a specified OKIT json file (okit.json) and converts it to a number terraform or ansible 
files that can then be used with the appropriate DevOps tooling to build the artifacts defined within the json file.
```bash
# Generate Terraform
python3 generate.py -f okit.json -d destination_directory -t

# Generate Ansible
python3 generate.py -f okit.json -d destination_directory -a

# Generate Terraform & Ansible
python3 generate.py -f okit.json -d destination_directory -t -a
```
#### Generate TF from OCI
This test program will connect to OCI with the default configuration and query based on the specified filter (-c) then generate
appropriate yerraform files below the specified directory.
```bash
cd test/unit.test
python3 ./generateTFfromOCI.py -d . -c Stefan
```

## Development
All currently active / planned development is documented / being tracked in the internal Jira ticket 
[OKIT OCI Capture / DevOps / Visualisation Tool](http://ateam-engage.us.oracle.com/jira/browse/INT-2168). 
Before starting any new development work the Jira should be checked to see if anyone else is working on the same area. 
If this is new development then a new sub task should be created to document the artifact being added.

The following worked example will take you through the required steps to add a new artifact to OKIT. This will be based 
on adding Block Storage Volumes to OKIT. Adding an artifact to OKIT will require a number files to be created and a few 
modified the following steps will document the procedure specifying where the files will need to be created and the names 
to be used.

### Adding an Artifact
The following files will need to be created and the directories specified are relative to the project root. You will notice 
that the files have a specfic naming convention and this is important because it allows the wrapper code to work with the
minimum of cross file editing / multi developer editing. As a result it greatly simplifies the addition of new artifacts 
to the system. 

- New Files
    - Frontend
        - **[Palette SVG](palette-svg)**                             : okitweb/static/okit/palette/*Block_Storage_Volume*.svg
        - **[Artifact Javascript](artifact-javascript)**             : okitweb/static/okit/js/oci_assets/*block_storage_volume*.js
        - **[Properties HTML](properties-html)**                     : okitweb/templates/okit/propertysheets/*block_storage_volume*.html
    - Backend
        - **[Python OCI Facade](python-oci-facade)**                 : visualiser/facades/oci*BlockStorageVolume*.py
        - **[Terraform Jinja2 Template](terraform-jinja2-template)** : visualiser/templates/terraform/*block_storage_volume*.jinja2
        - **[Ansible Jinja2 Template](ansible-jinja2-template)**     : visualiser/templates/ansible/*block_storage_volume*.jinja2
- Updated Files
    - Frontend
        - **[Designer Javascript](designer-javascript)**             : okitweb/static/okit/js/okit_designer.js
        - **[Flask Web Dersiner Python](flask-web-designer-python)** : okitweb/okitWebDesigner.py
    - Backend
        - **[Python OCI Query](python-oci-query)**                   : visualiser/common/ociQuery.py
        - **[Python Generator](python-generator)**                   : visualiser/generators/ociGenerator.py

#### Naming Convention
All files associated with an artifict will have file names based on the artifact. If we take the ***Block Storage Volume***
artifact as an example it can be seen, from above, that all files are named in the same fashion with the exception of the
palette SVG file. 

All files must be named as per artifact name with the spaces replaced by underscores and converted to lowercase. The exception 
to this is the palette SVG where title case should be used rether than lower case. The reason for this is that the palette
file name will be manipulated (removing the underscore) and used to dynamically reference all Javascript function names.  

#### Palette SVG
The palette svg defines the icon that will be displayed in the Drag & Drop palette. A number of existing SVG files can be
downloaded from the confluence page [OCI Icon Set draw.io Stencils](https://confluence.oci.oraclecorp.com/pages/viewpage.action?spaceKey=~scross&title=OCI+Icon+Set+draw.io+Stencils).

#### Artifact Javascript
The artifact javascript file is the key files for the BUI specifying all core code for the creation, drawing and querying 
of the artifact. Each file has a standard set of variable definitions and function definitions which again are based on 
the name of the artifact as follows.

##### Standard Definitions
```javascript
console.log('Loaded Block Storage Javascript');

/*
** Set Valid drop Targets
 */
asset_drop_targets[block_storage_volume_artifact] = [compartment_artifact];
asset_connect_targets[block_storage_volume_artifact] = [instance_artifact];
asset_add_functions[block_storage_volume_artifact] = "addBlockStorageVolume";
asset_delete_functions[block_storage_volume_artifact] = "deleteBlockStorageVolume";

const block_storage_volume_stroke_colour = "#F80000";
const block_storage_volume_query_cb = "block-storage-volume-query-cb";
let block_storage_volume_ids = [];
let block_storage_volume_count = 0;
```
Within this section we will define the target artifact where the new can be dropped, connected to and functions defining how the 
artifact can be added, deleted and updated. For example the **Block Storage Volume** can be dropped on the *Compartment*
and connected to an *Instance*. 

***Note***: The block_storage_volume_artifact, compartment_artifact and instance_artifact constants are defined in the Designer Javascript.  

Additionally we define the stroke colour for the bounding rectangle used to display the artifact.

##### Clear Function
```javascript
/*
** Reset variables
 */

function clearBlockStorageVolumeVariables() {
    block_storage_volume_ids = [];
    block_storage_volume_count = 0;
}
```
Simple function to reset artifact specific variables.

##### Add Function
```javascript
/*
** Add Asset to JSON Model
 */
function addBlockStorageVolume(parent_id, compartment_id) {
    let id = 'okit-' + block_storage_volume_prefix + '-' + uuidv4();
    console.log('Adding ' + block_storage_volume_artifact + ' : ' + id);

    // Add Virtual Cloud Network to JSON

    if (!OKITJsonObj.hasOwnProperty('block_storage_volumes')) {
        OKITJsonObj['block_storage_volumes'] = [];
    }

    // Add id & empty name to id JSON
    okitIdsJsonObj[id] = '';
    block_storage_volume_ids.push(id);

    // Increment Count
    block_storage_volume_count += 1;
    let block_storage_volume = {};
    block_storage_volume['compartment_id'] = parent_id;
    block_storage_volume['availability_domain'] = '1';
    block_storage_volume['id'] = id;
    block_storage_volume['display_name'] = generateDefaultName(block_storage_volume_prefix, block_storage_volume_count);
    block_storage_volume['size_in_gbs'] = 1024;
    block_storage_volume['backup_policy'] = 'bronze';
    OKITJsonObj['block_storage_volumes'].push(block_storage_volume);
    okitIdsJsonObj[id] = block_storage_volume['display_name'];
    //console.log(JSON.stringify(OKITJsonObj, null, 2));
    displayOkitJson();
    drawBlockStorageVolumeSVG(block_storage_volume);
    loadBlockStorageVolumeProperties(id);
}
```
This function is used to add a new json element to the OKIT json structure. The elements within this json will match those 
that are returned from querying OCI. All artifacts will be contained within a top level list with a name that matches that
of the artifact (e.g. block_storage_volumes). Once that new json element has been added it will be drawn on the SVG canvas.

##### Delete Function
```javascript
/*
** Delete From JSON Model
 */

function deleteBlockStorageVolume(id) {
    console.log('Delete ' + block_storage_volume_artifact + ' : ' + id);
    // Remove SVG Element
    d3.select("#" + id + "-svg").remove()
    // Remove Data Entry
    for (let i=0; i < OKITJsonObj['block_storage_volumes'].length; i++) {
        if (OKITJsonObj['block_storage_volumes'][i]['id'] == id) {
            OKITJsonObj['block_storage_volumes'].splice(i, 1);
        }
    }
    // Remove Instance references
    if ('instances' in OKITJsonObj) {
        for (let instance of OKITJsonObj['instances']) {
            for (let i=0; i < instance['block_storage_volume_ids'].length; i++) {
                if (instance['block_storage_volume_ids'][i] == id) {
                    instance['block_storage_volume_ids'].splice(i, 1);
                }
            }
        }
    }
}
```
Function used to remove artifact and delete any references within linked artifacts.

##### Draw SVG
```javascript
/*
** SVG Creation
 */
function drawBlockStorageVolumeSVG(block_storage_volume) {
    let parent_id = block_storage_volume['compartment_id'];
    let id = block_storage_volume['id'];
    let compartment_id = block_storage_volume['compartment_id'];
    console.log('Drawing ' + block_storage_volume_artifact + ' : ' + id);
    if (compartment_bui_sub_artifacts.hasOwnProperty(parent_id)) {
        if (!compartment_bui_sub_artifacts[parent_id].hasOwnProperty('block_storage_position')) {
            compartment_bui_sub_artifacts[parent_id]['block_storage_position'] = 0;
        }
        let position = compartment_bui_sub_artifacts[parent_id]['block_storage_position'];
        let svg_x = 0; //(icon_width / 4);
        let svg_y = Math.round((icon_height * 3 / 4) + (icon_height * position) + (vcn_icon_spacing * position));
        let data_type = block_storage_volume_artifact;

        // Increment Icon Position
        compartment_bui_sub_artifacts[parent_id]['block_storage_position'] += 1;

        let parent_svg = d3.select('#' + parent_id + "-svg");
        let svg = parent_svg.append("svg")
            .attr("id", id + '-svg')
            .attr("data-type", data_type)
            .attr("data-parentid", parent_id)
            .attr("title", block_storage_volume['display_name'])
            .attr("x", svg_x)
            .attr("y", svg_y)
            .attr("width", "100")
            .attr("height", "100");
        let rect = svg.append("rect")
            .attr("id", id)
            .attr("data-type", data_type)
            .attr("data-parentid", parent_id)
            .attr("title", block_storage_volume['display_name'])
            .attr("x", icon_x)
            .attr("y", icon_y)
            .attr("width", icon_width)
            .attr("height", icon_height)
            .attr("stroke", block_storage_volume_stroke_colour)
            .attr("stroke-dasharray", "1, 1")
            .attr("fill", "white")
            .attr("style", "fill-opacity: .25;");
        rect.append("title")
            .attr("data-type", data_type)
            .attr("data-parentid", parent_id)
            .text(block_storage_volume_artifact + ": " + block_storage_volume['display_name']);
        let g = svg.append("g")
            .attr("data-type", data_type)
            .attr("data-parentid", parent_id)
            .attr("transform", "translate(5, 5) scale(0.3, 0.3)");
        g.append("path")
            .attr("data-type", data_type)
            .attr("data-parentid", parent_id)
            .attr("class", "st0")
            .attr("d", "M172.6,88.4c-13.7-1.6-28-1.6-28.6-1.6c-0.6,0-14.8,0-28.6,1.6c-24,2.8-24.2,7.8-24.2,7.9v95.5c0,0,0.3,5,24.2,7.9c13.7,1.6,28,1.6,28.6,1.6c0.6,0,14.8,0,28.6-1.6c24-2.8,24.2-7.8,24.2-7.9V96.3C196.8,96.2,196.5,91.2,172.6,88.4z M137.2,180.7h-18.9v-18.9h18.9V180.7z M137.2,146.5h-18.9v-18.9h18.9V146.5z M168.1,180.7h-18.9v-18.9h18.9V180.7z M168.1,146.5h-18.9v-18.9h18.9V146.5z M192.8,104.1c-1.8,2.8-18.9,7.5-48.3,7.5c-29.4,0-46.5-4.7-48.3-7.5c0,0,0,0,0,0c1.7-2.8,18.8-7.6,48.3-7.6C174,96.5,191.1,101.2,192.8,104.1C192.8,104.1,192.8,104.1,192.8,104.1z");

        let boundingClientRect = rect.node().getBoundingClientRect();
        /*
         Add click event to display properties
         Add Drag Event to allow connector (Currently done a mouse events because SVG does not have drag version)
         Add dragevent versions
         Set common attributes on svg element and children
         */
        svg.on("click", function () {
            loadBlockStorageVolumeProperties(id);
            d3.event.stopPropagation();
        })
            .on("mousedown", handleConnectorDragStart)
            .on("mousemove", handleConnectorDrag)
            .on("mouseup", handleConnectorDrop)
            .on("mouseover", handleConnectorDragEnter)
            .on("mouseout", handleConnectorDragLeave)
            .on("dragstart", handleConnectorDragStart)
            .on("drop", handleConnectorDrop)
            .on("dragenter", handleConnectorDragEnter)
            .on("dragleave", handleConnectorDragLeave)
            .on("contextmenu", handleContextMenu)
            .attr("data-type", data_type)
            .attr("data-okit-id", id)
            .attr("data-parentid", parent_id)
            .attr("data-compartment-id", compartment_id)
            .attr("data-connector-start-y", boundingClientRect.y + boundingClientRect.height)
            .attr("data-connector-start-x", boundingClientRect.x + (boundingClientRect.width/2))
            .attr("data-connector-end-y", boundingClientRect.y + boundingClientRect.height)
            .attr("data-connector-end-x", boundingClientRect.x + (boundingClientRect.width/2))
            .attr("data-connector-id", id)
            .attr("dragable", true)
            .selectAll("*")
                .attr("data-type", data_type)
                .attr("data-okit-id", id)
                .attr("data-parentid", parent_id)
                .attr("data-compartment-id", compartment_id)
                .attr("data-connector-start-y", boundingClientRect.y + boundingClientRect.height)
                .attr("data-connector-start-x", boundingClientRect.x + (boundingClientRect.width/2))
                .attr("data-connector-end-y", boundingClientRect.y + boundingClientRect.height)
                .attr("data-connector-end-x", boundingClientRect.x + (boundingClientRect.width/2))
                .attr("data-connector-id", id)
                .attr("dragable", true);
    } else {
        console.log(parent_id + ' was not found in compartment sub artifacts : ' + JSON.stringify(compartment_bui_sub_artifacts));
    }
}
``` 
Draws the artifact on the SVG canvas as parted of the dropped component. All artifacts are contained within there own svg
element because we can then drop, where appropriate, other arifacts on them and they become self contained. Once draw we
will add a click event to display the properties associated with this artifact. If the artifact can be connected to another
then we will also add the standard drag & drop handlers (Mouse Handlers are added as well because SVG does not support
standard HTML drag & drop events).

##### Load Property Sheet
```javascript
/*
** Property Sheet Load function
 */
function loadBlockStorageVolumeProperties(id) {
    $("#properties").load("propertysheets/block_storage_volume.html", function () {
        if ('block_storage_volumes' in OKITJsonObj) {
            console.log('Loading ' + block_storage_volume_artifact + ' : ' + id);
            let json = OKITJsonObj['block_storage_volumes'];
            for (let i = 0; i < json.length; i++) {
                let block_storage_volume = json[i];
                if (block_storage_volume['id'] == id) {
                    block_storage_volume['virtual_cloud_network'] = okitIdsJsonObj[block_storage_volume['vcn_id']];
                    $("#virtual_cloud_network").html(block_storage_volume['virtual_cloud_network']);
                    $('#display_name').val(block_storage_volume['display_name']);
                    $('#availability_domain').val(block_storage_volume['availability_domain']);
                    $('#size_in_gbs').val(block_storage_volume['size_in_gbs']);
                    $('#backup_policy').val(block_storage_volume['backup_policy']);
                    // Add Event Listeners
                    addPropertiesEventListeners(block_storage_volume, []);
                    break;
                }
            }
        }
    });
}
```
When the user clicks on the drawn SVG artifact this load function will be called. It will load the artifact specific 
properties sheet into the "properties" pane and then load each of the form fields with the data from the appropriate 
json element.

##### Query OCI
```javascript
/*
** Query OCI
 */

function queryBlockStorageVolumeAjax(compartment_id) {
    console.log('------------- queryBlockStorageVolumeAjax --------------------');
    let request_json = {};
    request_json['compartment_id'] = compartment_id;
    if ('virtual_cloud_network_filter' in okitQueryRequestJson) {
        request_json['virtual_cloud_network_filter'] = okitQueryRequestJson['virtual_cloud_network_filter'];
    }
    $.ajax({
        type: 'get',
        url: 'oci/artifacts/BlockStorageVolume',
        dataType: 'text',
        contentType: 'application/json',
        //data: JSON.stringify(okitQueryRequestJson),
        data: JSON.stringify(request_json),
        success: function(resp) {
            let response_json = JSON.parse(resp);
            OKITJsonObj['block_storage_volumes'] = response_json;
            let len =  response_json.length;
            for(let i=0;i<len;i++ ){
                console.log('queryBlockStorageVolumeAjax : ' + response_json[i]['display_name']);
            }
            redrawSVGCanvas();
            $('#block-storage-volume-query-cb').prop('checked', true);
            hideQueryProgressIfComplete();
        },
        error: function(xhr, status, error) {
            console.log('Status : '+ status)
            console.log('Error : '+ error)
        }
    });
}
```
Uses Ajax to call the flask url to initiate an asynchronous query of OCI to retrieve all artifacts and then redraw the 
svg canvas. On completion it will set the query progress for this artifact as complete.

##### Read Function
```javascript
$(document).ready(function() {
    clearBlockStorageVolumeVariables();

    let body = d3.select('#query-progress-tbody');
    let row = body.append('tr');
    let cell = row.append('td');
    cell.append('input')
        .attr('type', 'checkbox')
        .attr('id', block_storage_volume_query_cb);
    cell.append('label').text(block_storage_volume_artifact);
});
```
Clear the artifact variables and add the query checkbox to the query progress table.

#### Properties HTML
#### Python OCI Facade
#### Terraform Jinja2 Template
#### Ansible Jinja2 Template
#### Designer Javascript
#### Flask Web Dersiner Python
#### Python OCI Query
#### Python Generator

## Contributing

Bug reports, enhancement request and pull requests are welcome on the OraHub at [okit.oci.web.designer/issues](https://orahub.oraclecorp.com/cloud-tools-ateam/okit.oci.web.designer/issues)

## Examples

## Cleanup

