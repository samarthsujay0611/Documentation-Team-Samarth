# LDAP Setup on Podman

## Environment Information:

### Server Info:

- **OS Version:** Ubuntu 20.04.6 LTS (Focal Fossa)
- **Podman Version:** 3.4.2

### Client Info:

- **OS Version:** Ubuntu 20.04.6 LTS 
- **LDAP Version:** 3
- **Apache Directory Studio Version:** 2.0.0.v20210717-M17

## List of Tools:

- Podman
- LDAP
- Apache Directory Studio
- Java (JDK)

## LDAP:

The Lightweight Directory Access Protocol (LDAP) is a communication protocol used to access directory servers, storing, updating, and retrieving data from a directory structure.

### Installation Steps:

1. Add Podman repository:

   ```bash
   echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_20.04/ /" | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
   ```

2. Install curl:

   ```bash
   sudo apt install curl
   ```

3. Import the repository key:

   ```bash
   curl -L "https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_20.04/Release.key" | sudo apt-key add -
   ```

4. Update the system:

   ```bash
   sudo apt update
   ```

5. Install Podman:

   ```bash
   sudo apt install podman
   ```

6. Check Podman version:

   ```bash
   podman -v
   ```

7. Setup and configuration:

   ```bash
   mkdir -p 389ds/data
   cd 389ds/data
   vim ldap.sh
   ```

   ```bash
   #!/bin/bash
   podman pod create --name ldap389 --publish 3389:3389 --publish 3636:3636
   podman run -dt \
   --pod ldap389 \
   --name 389ds-ldap \
   -v /home/local/389ds/data:/data \
   -e DS_SUFFIX=dc=keenable,dc=in \
   -e DS_DM_PASSWORD=Samarth \
   docker.io/389ds/dirsrv
   ```

   ```bash
   sudo chmod 777 ldap.sh
   sh -x ldap.sh
   ```

   ```bash
   podman ps -a --pod
   podman exec -it 389ds-ldap bash
   ```

8. Create a backend suffix list:

   ```bash
   dsconf -D "cn=Directory Manager" ldap://localhost:3389 backend create --suffix="dc=keenable,dc=in" --be-name="keenable"
   ```

9. Install LDAP utilities:

   ```bash
   sudo apt install ldap-utils
   ```

10. Set up Apache Directory Studio for LDAP DB UI:

   a. Install Apache Directory Studio tar file and extract in the directory.

   b. Un-tar the downloaded tar file:

      ```bash
      tar -xvf ApacheDirectoryStudio-2.0.0.v20210717-M17-linux.gtk.x86_64.tar.gz
      ```

   c. Install OpenJDK:

 ```bash
    sudo apt install openjdk11-jdk
 ```

      Check Java version:

      ```bash
      java -v
      ```

d. Open Apache Directory Studio and configure LDAP connection.

## Part 2:

### Create Organization_unit LDIF File:

```bash
vim organisation.ldif
```

```ldif
# LDIF File for Organization Units
dn: dc=keenable,dc=in
objectClass: top
objectClass: domain
dc: keenable

dn: ou=Dev,dc=keenable,dc=in
objectClass: top
objectClass: organizationalUnit
ou: Dev

dn: ou=Support,dc=keenable,dc=in
objectClass: top
objectClass: organizationalUnit
ou: Support

dn: ou=POC,dc=keenable,dc=in
objectClass: top
objectClass: organizationalUnit
ou: POC

dn: ou=Document,dc=keenable,dc=in
objectClass: top
objectClass: organizationalUnit
ou: Document

dn: ou=Observability,dc=keenable,dc=in
objectClass: top
objectClass: organizationalUnit
ou: Observability
```

Run this command to add `organisation.ldif`:

```bash
ldapadd -a -c -xH ldap://localhost:3389 -D "cn=Directory Manager" -w Samarth -f organisation.ldif
```

### Create Group LDIF File:

```bash
vim group.ldif
```

```ldif
# LDIF File for Groups
dn: uid=001,ou=dev,dc=keenable,dc=in
objectClass: top
objectClass: inetOrgPerson
objectClass: customEmployee
cn: Samarth
sn: saujay
uid: 001
EmployeeCode: 101
# Add other attributes as needed...
```

Run this command to add `group.ldif`:

```bash
ldapadd -a -c -xH ldap://localhost:3389 -D "cn=Directory Manager" -w Samarth -f group.ldif
```

### Run LDAP Commands:

```bash
ldapsearch -o ldif-wrap=no -x -H ldap://localhost:3389 -D "cn=Directory Manager" -w "Samarth" -b "cn=schema" "(objectClass=subSchema)" -s sub "objectClasses"
```

### Create Custom Attribute:

```bash
vim custom_attribute.ldif
```

```ldif
# LDIF File for Custom Attributes
dn: cn=schema
changetype: modify
add: attributeTypes
attributetypes: (emp_code-oid NAME 'EmployeeCode' DESC 'EmployeeCode' EQUALITY caseIgnoreMatch SUBSTR caseExactSubstringsMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.15{10} SINGLE-VALUE X-ORIGIN 'user defined' )
# Add other custom attributes...
```

Run this command to add `custom_attribute.ldif`:

```bash
ldapadd -a -c -xH ldap://localhost:3389 -D "cn=Directory Manager" -w Samarth -f custom_attribute.ldif
```

### Create Object Class File:

```bash
vim object_class.ldif
```

```ldif
# LDIF File for Object Classes
dn: cn=schema
changetype: modify
add: objectClasses
objectClasses: ( customEmployee-oid NAME 'customEmployee' SUP top AUXILIARY DESC 'Custom Employee