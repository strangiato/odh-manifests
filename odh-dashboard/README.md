# Dashboard

The Open Data Hub Dashboard component installs a UI which 

- Shows what's installed
- Show's what's available for installation
- Links to component UIs
- Links to component documentation

For more information, visit the project [GitHub repo](https://github.com/opendatahub-io/odh-dashboard).

### Folders
1. base: contains all the necessary yaml files to install the dashboard
2. overlays/authentication: Contains the necessary yaml files to install the
   Open Data Hub Dashboard configured to require users to authenticate to the
   OpenShift cluster before they can access the service

### Installation
```
  - kustomizeConfig:
      repoRef:
        name: manifests
        path: odh-dashboard
    name: odh-dashboard
```

If you would like to configure the dashboard to require authentication:
```
  - kustomizeConfig:
      overlays:
        - authentication
      repoRef:
        name: manifests
        path: odh-dashboard
    name: odh-dashboard
```

### Admin Control Panel

The ODH Dashboard has several features only accessible to admin users which requires some configuration to access.

To access the admin control panel in the dashboard it is highly recommended that you deploy the dashboard with the authentication overlay.

By default the ODH Dashboard utilizes an OpenShift Group object named `odh-admins` to grant access to the admin panel.  That group is not automatically deployed by ODH.  You can manually create that object:

```yaml
kind: Group
apiVersion: user.openshift.io/v1
metadata:
  name: odh-admins
users:
  - user1
  - user2
```

#### Configuring A Custom Admin Group

If you wish to use an existing group or create a new custom group object you can add the following paramaters:

```yaml
  - kustomizeConfig:
      overlays:
        - authentication
      parameters:
        - name: odh_dashboard_admin_groups
          value: <my-group>
      repoRef:
        name: manifests
        path: odh-dashboard
    name: odh-dashboard
```
