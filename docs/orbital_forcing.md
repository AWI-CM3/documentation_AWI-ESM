# Changing Greenhouse Gas and Orbital Forcing
```{article-info}
:avatar: https://secure.gravatar.com/avatar/709ea66dc102e6bc4547032f85ff6c95 
:avatar-link: mailto:paul.gierz@awi.de 
:avatar-outline: muted
:author: Paul Gierz 
:date: June 2022
:read-time: "10 min read and hands-on"
:class-container: sd-p-2 sd-outline-muted sd-rounded-1
```

## Greenhouse Gases
A common experiment with Earth System Models is to modify the greenhouse gas
concentrations. Here we describe how to set up these types of experiments. 

### Determining appropriate concentrations

Greenhouse gas values are typically reconstructed from ice core records. Several key records that are often used are.....

### Setting up your simulation

`````{tab-set}
````{tab-item} ECHAM 6.3.05p2
For `ECHAM 6.3.05p2`, you need to modify entries in `namelist.echam`. In the
`esm-tools` framework, this can be done by adding the following entries to your
run configuration.

```diff
 echam:
+    add_namelist_changes:
+        namelist.echam:
+            co2vmr: <SOME VALUE>
+            ch4vmr: <SOME VALUE>
+            n2ovmr: <SOME VALUE>
```
````
````{tab-item} Open IFS
```{warning}
Still under construction
```
````
`````

## Orbital Configuration

In paleoclimate contexts, it is often required to not only modify the
greenhouse gas concentrations, but also to change the orbital configuration of
the Earth around the Sun. Three primary factors control the orbit, firstly the
eccentrity of the eliptical orbit, Earth's axial tilt; namely the obliquity of
the ecliptic, and finally the direction in which the axis of rotation is
pointed, known as precession.

### Determining appropriate orbital settings
blah blah look up in Berger

### Setting up your simulation

`````{tab-set}
````{tab-item} ECHAM 6.3.05p2
For `ECHAM 6.3.05p2`, you need to modify entries in `namelist.echam`. In the
`esm-tools` framework, this can be done by adding the following entries to your
run configuration.

```diff
 echam:
+    add_namelist_changes:
+        namelist.echam:
+            cecc: <SOME VALUE>
+            cobld: <SOME VALUE>
+            clonp: <SOME VALUE>
```
````
````{tab-item} Open IFS
```{warning}
Still under construction
```
````
`````


