# AWS User Store Extension for WSO2 Identity Server

The AWS user store extension allows you to use [AWS Cloud Directory] (https://docs.aws.amazon.com/clouddirectory/latest/developerguide/what_is_cloud_directory.html) as
a user store in WSO2 Identity Server using [REST API](https://docs.aws.amazon.com/directoryservice/latest/APIReference/welcome.html).
AWS Cloud Directory is a specialized graph-based directory store that provides a foundational building block for developers. With AWS Cloud Directory, you can organise directory objects into multiple hierarchies to support many organizational pivots and relationships across directory information.
You can use the AWS user store extension as the primary or secondary user store in WSO2 Identity Server. The AWS user store extension uses the `org.wso2.carbon.aws.user.store.mgt.AWSUserStoreManager` class to configure AWS user store manager.

> **NOTE**: AWS user store extension is compatible with WSO2 Identity Server 5.5.0, 5.6.0 as well as 5.7.0.

The following topics provide information on how you can configure the AWS user store extension with WSO2 Identity Server and then use AWS as the primary or secondary user store in WSO2 Identity Server:

- [Prerequisites](#prerequisites)
- [Adding the AWS user store extension to WSO2 Identity Server](#adding-the-aws-user-store-extension-to-wso2-identity-server)
- [Configuring AWS as the secondary user store](#configuring-aws-as-the-secondary-user-store)
- [Configuring AWS as the primary user store](#configuring-aws-as-the-primary-user-store)
- [AWS user store manager properties](#aws-user-store-manager-properties)

## Prerequisites

1. Create a cloud directory by uploading the schema for the objects via the AWS console. You can take a look at following sample schema to configure the sample user store configuration:

```json
{
 "facets": {
   "ROLES": {
     "facetAttributes": {
       "RoleName": {
         "attributeDefinition": {
           "attributeType": "STRING",
           "isImmutable": false
         },
         "requiredBehavior": "REQUIRED_ALWAYS"
       },
       "MemberOf": {
         "attributeDefinition": {
           "attributeType": "STRING",
           "isImmutable": false
         },
         "requiredBehavior": "NOT_REQUIRED"
       }
     },
     "objectType": "LEAF_NODE"
   },
   "USERS": {
     "facetAttributes": {
       "UserName": {
         "attributeDefinition": {
           "attributeType": "STRING",
           "isImmutable": false
         },
         "requiredBehavior": "REQUIRED_ALWAYS"
       },
       "Password": {
         "attributeDefinition": {
           "attributeType": "STRING",
           "isImmutable": false
         },
         "requiredBehavior": "REQUIRED_ALWAYS"
       },
       "Member": {
         "attributeDefinition": {
           "attributeType": "STRING",
           "isImmutable": false
         },
         "requiredBehavior": "NOT_REQUIRED"
       }
     },
     "objectType": "LEAF_NODE"
   }
 }
}
```

>**NOTE**: If you are going to maintain a set of claims such as *givenName*, *mail*, *sn*, and *profileConfiguration* in the user profile, you need to update the sample schema above as follows:

```json
 {
  "facets": {
    "ROLES": {
      "facetAttributes": {
        "RoleName": {
          "attributeDefinition": {
            "attributeType": "STRING",
            "isImmutable": false
          },
          "requiredBehavior": "REQUIRED_ALWAYS"
        },
        "MemberOf": {
          "attributeDefinition": {
            "attributeType": "STRING",
            "isImmutable": false
          },
          "requiredBehavior": "NOT_REQUIRED"
        }
      },
      "objectType": "LEAF_NODE"
    },
    "USERS": {
      "facetAttributes": {
        "UserName": {
          "attributeDefinition": {
            "attributeType": "STRING",
            "isImmutable": false
          },
          "requiredBehavior": "REQUIRED_ALWAYS"
        },
        "Password": {
          "attributeDefinition": {
            "attributeType": "STRING",
            "isImmutable": false
          },
          "requiredBehavior": "REQUIRED_ALWAYS"
        },
        "Member": {
          "attributeDefinition": {
            "attributeType": "STRING",
            "isImmutable": false
          },
          "requiredBehavior": "NOT_REQUIRED"
        },
        "givenName": {
          "attributeDefinition": {
            "attributeType": "STRING",
            "isImmutable": false
          },
          "requiredBehavior": "NOT_REQUIRED"
        },
        "mail": {
          "attributeDefinition": {
            "attributeType": "STRING",
            "isImmutable": false
          },
          "requiredBehavior": "NOT_REQUIRED"
        },
        "sn": {
          "attributeDefinition": {
            "attributeType": "STRING",
            "isImmutable": false
          },
          "requiredBehavior": "NOT_REQUIRED"
        },
        "profileConfiguration": {
          "attributeDefinition": {
            "attributeType": "STRING",
            "isImmutable": false
          },
          "requiredBehavior": "NOT_REQUIRED"
        }
      },
      "objectType": "LEAF_NODE"
    }
  }
 }
```
2. Obtain the following property values to use in the AWS user store configuration:
- DirectoryArn 
- SchemaArn
- FacetNameOfUser
- FacetNameOfRole
- UserNameAttribute 
- PasswordAttribute 
- MembershipAttribute 
- RoleNameAttribute 
- MemberOfAttribute

### Adding the AWS user store extension to WSO2 Identity Server

Follow the steps below to add the AWS user store extension to WSO2 Identity Server and confirm that you have successfully added the AWS user store extension to WSO2 Identity Server:

1. Go to [WSO2 store](https://store.wso2.com/store/assets/isconnector/details/f568ff1e-d746-47ce-b8cb-85f55e1e08a7) and download the AWS user store extension.
2. Copy the downloaded jar to the `<IS_HOME>/repository/components/dropins` directory. Now you have added the AWS user store extension to WSO2 Identity Server.
3. Open a terminal, navigate to the `<IS_HOME>/bin` directory, and then execute `./wso2server.sh` to start WSO2 Identity server.
4. Access the Management Console via `https://localhost:9443/carbon`.
5. Click the **Main** tab on the Management Console, and then click **Add** under **User Stores**. This displays the **Add New User Store** screen.
6. Click the list of items related to **User Store Manager Class**. You will see **org.wso2.carbon.aws.user.store.mgt.AWSUserStoreManager** in the list. This confirms that you have successfully added the AWS user store extension to WSO2 Identity Server.

Now that you have successfully added the AWS user store extension to WSO2 Identity Server, you can go ahead and configure AWS as the secondary user store.

### Configuring AWS as the secondary user store

Follow the steps below to configure AWS as the secondary user store.

1. In the **Add New User Store** screen, select **org.wso2.carbon.aws.user.store.mgt.AWSUserStoreManager** as the **User Store Manager Class**.
2. Enter appropriate values in the **Domain Name** and **Description** fields.
3. Enter appropriate values for all the mandatory AWS user store manager properties. For information on each property, see [AWS user store manager properties](#aws-user-store-manager-properties).

### Configuring AWS as the primary user store

>**NOTE**: If you want to configure AWS as the primary user store in WSO2 Identity Server, there are additional configurations that you need to perform.

Follow the steps below to configure AWS as the primary user store in WSO2 Identity Server:

1. Follow steps 1 and 2 under [Adding the AWS user store extension to WSO2 Identity Server](#adding-the-aws-user-store-extension-to-wso2-identity-server).
2. Edit the `<IS_HOME>/repository/conf/user-mgt.xml` file and add the following configuration:

>**NOTE**: When you add the following configuration, be sure to specify applicable values for the `AccessKeyID` and `SecretAccessKey`.

#### user-mgt.xml

```xml
<UserStoreManager class="org.wso2.carbon.aws.user.store.mgt.AWSUserStoreManager">
    <Property name="TenantManager">org.wso2.carbon.user.core.tenant.JDBCTenantManager</Property>
    <Property name="AccessKeyID">xxxxxxxxxxxxxxxxxxxxxxxxx</Property>
    <Property name="SecretAccessKey">xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</Property>
    <Property name="Region">us-west-2</Property>
    <Property name="APIVersion">2017-01-11</Property>
    <Property name="PathToUsers">/com/users</Property>
    <Property name="PathToRoles">/com/roles</Property>
    <Property name="MembershipTypeOfRoles">link</Property>
    <Property name="DirectoryArn">arn:aws:clouddirectory:us-west-2:610968236798:directory/ASc_ZQAllU0Ot0_vpmXmwF4</Property>
    <Property name="SchemaArn">arn:aws:clouddirectory:us-west-2:610968236798:directory/ASc_ZQAllU0Ot0_vpmXmwF4/schema/userstoreSchema/1.0</Property>
    <Property name="FacetNameOfUser">USERS</Property>
    <Property name="FacetNameOfRole">ROLES</Property>
    <Property name="ReadOnly">false</Property>
    <Property name="ReadGroups">true</Property>
    <Property name="WriteGroups">true</Property>
    <Property name="Disabled">false</Property>
    <Property name="UserNameAttribute">UserName</Property>
    <Property name="PasswordAttribute">Password</Property>
    <Property name="MembershipAttribute">Member</Property>
    <Property name="RoleNameAttribute">RoleName</Property>
    <Property name="MemberOfAttribute">MemberOf</Property>
    <Property name="UserNameJavaRegEx">[a-zA-Z0-9._\-|//]{3,30}$</Property>
    <Property name="UserNameJavaScriptRegEx">^[\S]{3,30}$</Property>
    <Property name="UsernameJavaRegExViolationErrorMsg">Username pattern policy violated</Property>
    <Property name="PasswordJavaRegEx">^[\S]{5,30}$</Property>
    <Property name="PasswordJavaScriptRegEx">^[\S]{5,30}$</Property>
    <Property name="PasswordJavaRegExViolationErrorMsg">Password pattern policy violated.</Property>
    <Property name="RoleNameJavaRegEx">[a-zA-Z0-9._-|//]{3,30}$</Property>
    <Property name="RoleNameJavaScriptRegEx">^[\S]{3,30}$</Property>
    <Property name="PasswordHashMethod">PLAIN_TEXT</Property>
    <Property name="MaxUserNameListLength">100</Property>
    <Property name="MaxRoleNameListLength">100</Property>
</UserStoreManager>
```

##### Sample user store configurations and corresponding tree structures for AWS

**Scenario 1**: Let's take a look at how to use typed links to model relationships between different objects (i.e., Users, Roles).

Following is a sample user store configuration for the scenario:

        ``` xml
        <UserStoreManager class="org.wso2.carbon.aws.user.store.mgt.AWSUserStoreManager">
           <Property name="AccessKeyID">AGBMVJTFDRGJKOGFD</Property>
           <Property name="SecretAccessKey">xxxxxxxxxxxx</Property>
           <Property name="Region">us-west-2</Property>
           <Property name="APIVersion">2017-01-11</Property>
           <Property name="PathToUsers">/org/users</Property>
           <Property name="PathToRoles">/org/roles</Property>
           <Property name="MembershipTypeOfRoles">link</Property>
           <Property name="DirectoryArn">arn:aws:clouddirectory:us-west-2:61898:directory/AVLHa3w</Property>
           <Property name="SchemaArn">arn:aws:clouddirectory:us-west-2:61898:directory/AVLHa3w/schema/schema/1.0</Property>
           <Property name="FacetNameOfUser">USERS</Property>
           <Property name="FacetNameOfRole">ROLES</Property>
           <Property name="ReadGroups">true</Property>
           <Property name="WriteGroups">true</Property>
           <Property name="Disabled">false</Property>
           <Property name="UserNameAttribute">UserName</Property>
           <Property name="PasswordAttribute">Password</Property>
           <Property name="MembershipAttribute"/>
           <Property name="RoleNameAttribute">RoleName</Property>
           <Property name="MemberOfAttribute"/>
           <Property name="UserNameJavaRegEx">[a-zA-Z0-9._\-|//]{3,30}$</Property>
           <Property name="UserNameJavaScriptRegEx">^[\S]{3,30}$</Property>
           <Property name="UsernameJavaRegExViolationErrorMsg">Username pattern policy violated.</Property>
           <Property name="PasswordJavaRegEx">^[\S]{5,30}$</Property>
           <Property name="PasswordJavaScriptRegEx">^[\S]{5,30}$</Property>
           <Property name="PasswordJavaRegExViolationErrorMsg">Password pattern policy violated.</Property>
           <Property name="RoleNameJavaRegEx">[a-zA-Z0-9._-|//]{3,30}$</Property>
           <Property name="RoleNameJavaScriptRegEx">^[\S]{3,30}$</Property>
           <Property name="PasswordHashMethod">PLAIN_TEXT</Property>
           <Property name="MaxUserNameListLength">100</Property>
           <Property name="MaxRoleNameListLength">100</Property>
        </UserStoreManager>
        ```
Based on the `MembershipTypeOfRoles` property in the above configuration, an end-user should use a link to model an ownership between the `Users` object and `Roles` object. Therefore, the directory structure should be similar to what is depicted in the following diagram:

![alt text](docs/images/image1.png)

For example, if you assign multiple roles such as Role1 and Role2 to User1, and you want to establish a relationship between the objects, you have to create the following typed links:

- User1 → Role1
- User1 → Role2

**Scenario 2**: Let's take a look at how you can maintain different object relationship details (i.e., Users, Roles) as an attribute inside the `Users` object and `Roles` object.

Following is a sample user store configuration for the scenario:

        ```xml
        <UserStoreManager class="org.wso2.carbon.aws.user.store.mgt.AWSUserStoreManager">
           <Property name="AccessKeyID">AGBMVJTFDRGJKOGFD</Property>
           <Property name="SecretAccessKey">xxxxxxxxxxx</Property>
           <Property name="Region">us-west-2</Property>
           <Property name="APIVersion">2017-01-11</Property>
           <Property name="PathToUsers">/com/example/users</Property>
           <Property name="PathToRoles">/com/example/roles</Property>
           <Property name="MembershipTypeOfRoles">attribute</Property>
           <Property name="DirectoryArn">arn:aws:clouddirectory:us-west-2:61898:directory/AVLHa3w</Property>
           <Property name="SchemaArn">arn:aws:clouddirectory:us-west-2:61898:directory/AVLHa3w/schema/schema/1.0</Property>
           <Property name="FacetNameOfUser">USERS</Property>
           <Property name="FacetNameOfRole">ROLES</Property>
           <Property name="ReadGroups">true</Property>
           <Property name="WriteGroups">true</Property>
           <Property name="Disabled">false</Property>
           <Property name="UserNameAttribute">UserName</Property>
           <Property name="PasswordAttribute">Password</Property>
           <Property name="MembershipAttribute">Member</Property>
           <Property name="RoleNameAttribute">RoleName</Property>
           <Property name="MemberOfAttribute">MemberOf</Property>
           <Property name="UserNameJavaRegEx">[a-zA-Z0-9._\-|//]{3,30}$</Property>
           <Property name="UserNameJavaScriptRegEx">^[\S]{3,30}$</Property>
           <Property name="UsernameJavaRegExViolationErrorMsg">Username pattern policy violated.</Property>
           <Property name="PasswordJavaRegEx">^[\S]{5,30}$</Property>
           <Property name="PasswordJavaScriptRegEx">^[\S]{5,30}$</Property>
           <Property name="PasswordJavaRegExViolationErrorMsg">Password pattern policy violated.</Property>
           <Property name="RoleNameJavaRegEx">[a-zA-Z0-9._-|//]{3,30}$</Property>
           <Property name="RoleNameJavaScriptRegEx">^[\S]{3,30}$</Property>
           <Property name="PasswordHashMethod">PLAIN_TEXT</Property>
           <Property name="MaxUserNameListLength">100</Property>
           <Property name="MaxRoleNameListLength">100</Property>
        </UserStoreManager>
        ```
        
Based on the `MembershipTypeOfRoles` property in the above configuration, an end-user should use an attribute to keep an ownership relationship between the `Users` object and `Roles` object. Therefore, the directory structure should be similar to what is depicted in the following diagram:

![alt text](docs/images/image2.png)

For example, if you assign multiple roles such as Role1 and Role2 to User1, then the relationship between the objects should be kept as an attribute inside the object itself (i.e., Using the `member` attribute in the `Users` object and `Memberof` attribute in the `Roles` object).

In the two scenarios described above, the additional attributes are kept inside each object as follows:

- The `Users` object will include `UserName`, `Password` and the set of claims.
- The `Roles` object will include `RoleName`.

#### AWS user store manager properties


| Property | Description |
| ------------- |-------------|
| AccessKeyID | The AWS access key ID |
| SecretAccessKey | The AWS secret access key |
| Region | The Region that is used when selecting a regional endpoint to make API requests |
| APIVersion | The cloud directory API version |
| PathToUsers| The path to identify the `Users` object in the tree structure. <br/><br/> A child link creates a parent–child relationship between the objects that it connects. For example, in the diagram that depicts scenario 1, the Users child link connects `Org` and `Users` objects. <br/><br/> Child links have names when they participate in defining the path of the object that the link points to. Therefore, to construct the path for the `Users` object, you need to use the link names from each parent–child link. Path selectors start with a slash (/) and the link names are separated by slashes (i.e., `/some/path` identifies the `Users` object based on path).
For example, if we consider the diagram that depicts scenario 1, `PathToUsers` will be `/org/users`.|
| PathToRoles| The path to identify the `Roles` object in the tree structure. <br/><br/> A child link creates a parent–child relationship between the objects it connects. For example, in the diagram that depicts scenario 1, the Roles child link connects `Org` and `Roles` objects. <br/><br/> Child links have names when they participate in defining the path of the object that the link points to. Therefore, to construct the path for the `Roles` object, use the link names from each parent–child link. Path selectors start with a slash (/) and link names are separated by slashes (i.e., `/some/path` identifies the `Roles` object based on path). 
For example, if we consider the diagram that depicts scenario 1, `PathToRoles` will be `/org/roles`.|
| MembershipTypeOfRoles | Indicates how you are going to maintain user and role object relationships. <br/><br/> Possible values are: `link`and `attribute`. <br/><br/> If you use link, you can establish a relationship between objects in Cloud Directory using typed links. You can then use these relationships to query for information. For example, to list the roles that are assigned to a particular user, to list the users who are assigned to a particular role. <br/><br/> If you use attribute, you can list the roles assigned to a particular user and list users who have a particular role. This maintains relationship between objects in an attribute inside the node using `MembershipAttribute` and `MemberOfAttribute`. |
| DirectoryArn | Directory in which the object will be created. |
| SchemaArn | Schema arn of the directory. |
| FacetNameOfUser | Facet name of the user object. |
| FacetNameOfRole | Facet name of the role object. |
| ReadGroups | This Indicates whether groups should be read from the user store. <br/><br/> Possible values: <br/> `true`: Read groups from user store. <br/> `false`: Do not read groups from user store |
| WriteGroups | Indicates whether to write groups to the user store. <br/><br/> Possible values: <br/> `true`: Write groups to user store. <br/> `false`: Don’t write groups to user store. Therefore, only internal roles can be created. |
| Disabled | Indicates whether to deactivate the user store. If you need to temporarily deactivate a user store, you can use this property. If you disable the user store using the disable option it also will set this parameter. <br/> Default: `false`. <br/><br/> Possible values: <br/> `true` : Disable user store temporarily. |
| UserNameAttribute | The name of the attribute used to identify the user name of the user entry. |
| PasswordAttribute | The name of the attribute used to identify the password of the user entry. |
| MembershipAttribute | This is an optional property. If you have specified a value for `MembershipTypeOfRoles`, you need to set this property and define the attribute that contain the distinguished names of user objects that are in a role. |
| RoleNameAttribute | The name of the attribute used to identify the role name of the role entry. |
| MemberOfAttribute | This is an optional property. <br/><br/> If you have specified a value for `MembershipTypeOfRoles`, you need to set this property and define the attribute that contain the distinguished names of role objects that the user is assigned to. |
| UserNameJavaRegEx | The regular expression used by back-end components for user name validation. By default, strings with non-empty characters that have a length of 3 to 30 are allowed. You can specify uppercase characters, lowercase characters, numbers and also ASCII values in the RegEx. <br/><br/> Default: `[a-zA-Z0-9._\-|//]{3,30}$`|
| UserNameJavaScriptRegEx | The regular expression used by front-end components for user name validation. <br/><br/> Default: `^[\S]{3,30}$`|
| UsernameJavaRegExViolationErrorMsg | Error message when the user name does not match `UsernameJavaRegEx` |
| PasswordJavaRegEx | The regular expression used by back-end components for password validation. By default, strings with non-empty characters that have a length of 5 to 30 are allowed. You can specify uppercase characters, lowercase characters, numbers and also ASCII values in the RegEx. <br/><br/> Default: `^[\S]{5,30}$` |
| PasswordJavaScriptRegEx | The regular expression used by front-end components for password validation. <br/><br/> Default: `^[\S]{5,30}$` |
| PasswordJavaRegExViolationErrorMsg | Error message when the password does not match `passwordJavaRegEx` |
| RoleNameJavaRegEx | The regular expression used by back-end components for role name validation. By default, strings with non-empty characters that have a length of 3 to 30 are allowed. You can specify uppercase characters, lowercase characters, numbers and also ASCII values in the RegEx. <br/><br/> Default: `[a-zA-Z0-9._-|//]{3,30}$` |
| RoleNameJavaScriptRegEx | The regular expression used by front-end components for role name validation. |
| PasswordHashMethod | The password hashing algorithm used to hash passwords before storing in the user store. |
| MaxUserNameListLength | Controls the number of users listed in the user store. This is useful when you have a large number of users and you do not want to list them all. You can set this property to 0 to displays all users. <br/><br/> Default: 100 |
| MaxRoleNameListLength | Controls the number of roles listed in the user store. This is useful when you have a large number of roles and do not want to list them all. You can set this property to 0 to displays all roles. <br/><br/> Default: 100 |

> **NOTE**: The `listObjectChildren` REST API operation is used to get the list of users/roles. This operation does not guarantee that all object children of `PathToUsers` or `PathToRoles` are `USERS` facet or `ROLES` facet. This operation also maintains the link name of each object as object name. Therefore, to ensure that all object children are `USERS` facet or `ROLES` facet, and to get the object name from the object instead of getting the link name, it is necessary to have other REST API calls with the listObjectAttributes operation. The additional network calls result in the limitations mentioned here.
