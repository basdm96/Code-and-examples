# Consolidated Source Index

This document combines multiple source files. Below is the index of the included files.

- [cookbooks.md](#cookbooks-md)
- [index.md](#index-md)
- [reference.md](#reference-md)
- [reference\Area.md](#reference-area-md)
- [reference\Data-Writers.md](#reference-data-writers-md)
- [reference\Dataclasses.md](#reference-dataclasses-md)
- [reference\Exceptions.md](#reference-exceptions-md)
- [reference\File-Processing.md](#reference-file-processing-md)
- [reference\Filters.md](#reference-filters-md)
- [reference\Geometry-Functions.md](#reference-geometry-functions-md)
- [reference\Handler-Processing.md](#reference-handler-processing-md)
- [reference\IO.md](#reference-io-md)
- [reference\Indexes.md](#reference-indexes-md)
- [reference\Replication.md](#reference-replication-md)
- [reference\Thread-Safety.md](#reference-thread-safety-md)
- [user_manual.md](#user_manual-md)
- [user_manual\01-First-Steps.md](#user_manual-01-first-steps-md)
- [user_manual\02-Extracting-Object-Data.md](#user_manual-02-extracting-object-data-md)
- [user_manual\03-Working-with-Geometries.md](#user_manual-03-working-with-geometries-md)
- [user_manual\04-Working-with-Filters.md](#user_manual-04-working-with-filters-md)
- [user_manual\05-Working-with-Handlers.md](#user_manual-05-working-with-handlers-md)
- [user_manual\06-Writing-Data.md](#user_manual-06-writing-data-md)
- [user_manual\07-Input-Formats-And-Other-Sources.md](#user_manual-07-input-formats-and-other-sources-md)
- [user_manual\08-Working-With-Change-Files.md](#user_manual-08-working-with-change-files-md)
- [user_manual\09-Working-With-History-Files.md](#user_manual-09-working-with-history-files-md)
- [user_manual\10-Replication-Tools.md](#user_manual-10-replication-tools-md)


---

## <a name="cookbooks-md"></a>File: cookbooks.md

# Overview

This section presents practical examples and use cases, where pyosmium can
come in handy. Each cookbook comes with three sections:

* __Task__ explains what should be achieved
* __Quick Solution__ gives you a ready to use script that solves the task
* __Background__ explains in detail what happens in the script and why it
  is done the way it is done. This should help you to write similar scripts
  on your own.

All cookbooks are available as Jupiter notebooks in the
[pyosmium source tree](https://github.com/osmcode/pyosmium/tree/master/docs/cookbooks/)


---

## <a name="index-md"></a>File: index.md

# Introduction

pyosmium is a library to efficiently read and process OpenStreetMap data files. It is based on the osmium library for reading and writing data and adds convenience functions that allow you to set up fast processing pipelines in Pythons that can handle even planet-sized data.

This manual comes in three parts:

* the [**User Manual**](user_manual.md) introduces the concepts and functionalities of pyosmium
* the [**Cookbook**](cookbooks.md) shows how to solve typical OSM data processing challenges with pyosmium
* the [**Reference**](reference.md) contains a complete list of classes and functions.

## Installation

The recommended way to install pyosmium is via pip:

    pip install osmium

Binary wheels are provided for all actively maintained Python versions on
Linux, MacOS and Windows 64bit.

### Installing from Source

To compile pyosmium from source or when installing it from the source wheel,
the following additional dependencies need to be available:

 * [libosmium](https://github.com/osmcode/libosmium) >= 2.16.0
 * [protozero](https://github.com/mapbox/protozero)
 * [cmake](https://cmake.org/)
 * [Pybind11](https://github.com/pybind/pybind11) >= 2.2
 * [expat](https://libexpat.github.io/)
 * [libz](https://www.zlib.net/)
 * [libbz2](https://www.sourceware.org/bzip2/)
 * [Boost](https://www.boost.org/) variant and iterator >= 1.41
 * [Python Requests](https://docs.python-requests.org/en/master/)
 * Python setuptools
 * a recent C++ compiler (Clang 3.4+, GCC 4.8+)

On Debian/Ubuntu-like systems, the following command installs all required
packages:

    sudo apt-get install python3-dev build-essential cmake libboost-dev \
                         libexpat1-dev zlib1g-dev libbz2-dev

libosmium, protozero and pybind11 are shipped with the source wheel. When
building from source, you need to download the source code and put it
in the subdirectory 'contrib'. Alternatively, if you want to put the sources
somewhere else, point pyosmium to the source code location by setting the
CMake variables `LIBOSMIUM_PREFIX`, `PROTOZERO_PREFIX` and
`PYBIND11_PREFIX` respectively.

To compile and install the bindings, run

    pip install [--user] .


---

## <a name="reference-md"></a>File: reference.md

# Overview

This section lists all functions and classes that pyosmium implements
for reference.

## Basic pyosmium types

::: osmium.BaseHandler
    options:
        heading_level: 3

::: osmium.BaseFilter
    options:
        heading_level: 3

### `HandlerLike` objects

Many functions in pyosmium take handler-like objects as a parameter. Next
to classes that derive from `BaseHandler` and `BaseFilter` you may also
hand in any object that has one of the handler functions `node()`, `way()`,
`relation()`, `area()`, or `changeset()` implemented.


---

## <a name="reference-area-md"></a>File: reference\Area.md

# Area building

::: osmium.area.AreaManager



---

## <a name="reference-data-writers-md"></a>File: reference\Data-Writers.md

# Data writers

::: osmium.SimpleWriter
::: osmium.WriteHandler
::: osmium.BackReferenceWriter
::: osmium.ForwardReferenceWriter



---

## <a name="reference-dataclasses-md"></a>File: reference\Dataclasses.md

# OSM data views

These classes expose the data from an OSM file to the Python scripts.
Objects of these classes are always _views_ unless stated otherwise.
This means that they are only valid as long as the view to an object is
valid.

## OSM types

::: osmium.osm.osm_entity_bits

## OSM primary objects

::: osmium.osm.OSMObject
    options:
        show_bases: False
::: osmium.osm.Node
::: osmium.osm.Way
::: osmium.osm.Relation
::: osmium.osm.Area
::: osmium.osm.Changeset

## Tag lists

::: osmium.osm.Tag
::: osmium.osm.TagList

## Node lists

::: osmium.osm.NodeRef
::: osmium.osm.NodeRefList
::: osmium.osm.WayNodeList
::: osmium.osm.InnerRing
::: osmium.osm.OuterRing

## Relation members

::: osmium.osm.RelationMember
::: osmium.osm.RelationMemberList

## Geometry types

::: osmium.osm.Box
::: osmium.osm.Location

## Mutable OSM objects

::: osmium.osm.mutable.OSMObject
::: osmium.osm.mutable.Node
::: osmium.osm.mutable.Way
::: osmium.osm.mutable.Relation


---

## <a name="reference-exceptions-md"></a>File: reference\Exceptions.md

# Exceptions

::: osmium.InvalidLocationError


---

## <a name="reference-file-processing-md"></a>File: reference\File-Processing.md

# Iterative Data Reading

::: osmium.FileProcessor
::: osmium.OsmFileIterator
::: osmium.BufferIterator
::: osmium.zip_processors




---

## <a name="reference-filters-md"></a>File: reference\Filters.md

# Filters

::: osmium.filter.EmptyTagFilter
::: osmium.filter.EntityFilter
::: osmium.filter.GeoInterfaceFilter
::: osmium.filter.IdFilter
::: osmium.filter.KeyFilter
::: osmium.filter.TagFilter



---

## <a name="reference-geometry-functions-md"></a>File: reference\Geometry-Functions.md

# Geometry builders and functions

::: osmium.geom.FactoryProtocol
::: osmium.geom.Coordinates
::: osmium.geom.GeoJSONFactory
::: osmium.geom.WKBFactory
::: osmium.geom.WKTFactory
::: osmium.geom.direction
::: osmium.geom.use_nodes

## Geometry functions

::: osmium.geom.lonlat_to_mercator
    options:
        heading_level: 3

::: osmium.geom.mercator_to_lonlat
    options:
        heading_level: 3


---

## <a name="reference-handler-processing-md"></a>File: reference\Handler-Processing.md

# Handlers and Handler Functions

::: osmium.SimpleHandler
::: osmium.MergeInputReader
::: osmium.NodeLocationsForWays

## Handler functions

::: osmium.apply
    options:
        heading_level: 3

::: osmium.make_simple_handler
    options:
        heading_level: 3


---

## <a name="reference-io-md"></a>File: reference\IO.md

# IO classes

::: osmium.io.File
::: osmium.io.FileBuffer
::: osmium.io.Header
::: osmium.io.Reader
::: osmium.io.Writer



---

## <a name="reference-indexes-md"></a>File: reference\Indexes.md

# Indexes

::: osmium.IdTracker
::: osmium.index.IdSet
::: osmium.index.LocationTable

## Index creation functions

::: osmium.index.map_types
    options:
        heading_level: 3

::: osmium.index.create_map
    options:
        heading_level: 3



---

## <a name="reference-replication-md"></a>File: reference\Replication.md

# Replication

## Replication server

::: osmium.replication.ReplicationServer
::: osmium.replication.OsmosisState
::: osmium.replication.DownloadResult


## Replication utils

::: osmium.replication.newest_change_from_file
::: osmium.replication.get_replication_header
::: osmium.replication.ReplicationHeader


---

## <a name="reference-thread-safety-md"></a>File: reference\Thread-Safety.md

# Thread safety

Object instances of pyosmium are not thread-safe to modify. If you share
objects like an index, you have to protect write accesses to these objects.
Concurrent reads are safe.

The library functions themselves are all reentrant and may be used safely from
different threads.

### Free-threaded Python

Starting with version 4.1, Pyosmium has experimental support for Python
runtimes with GIL disabled. See the
[Python Free-Threading Guide](https://py-free-threading.github.io/)
for more information.

The restrictions mentioned above still apply: write accesses on object need
to be protected by exclusive locks when using them in multi-threaded context.


---

## <a name="user_manual-md"></a>File: user_manual.md

# Overview

This user manual gives you an introduction on how to process OpenStreetMap
data

- [**First Steps**](user_manual/01-First-Steps.md)
  gives an overview of the OSM data model and how pyosmium processes the data
- [**Extracting Object Data**](user_manual/02-Extracting-Object-Data.md)
  looks into what data is contained inside an OSM object
- [**Working with Geometries**](user_manual/03-Working-with-Geometries.md)
  explains how to create points, line strings and polygons for OSM objects
- [**Working with Filters**](user_manual/04-Working-with-Filters.md)
  introduces how to select the right data to process
- [**Working with Handlers**](user_manual/05-Working-with-Handlers.md)
  shows how to work with a callback-based approach for processing data
- [**Writing data**](user_manual/06-Writing-Data.md)
  explains how to create a new OSM file
- [**Input Formats and Other Sources**](user_manual/07-Input-Formats-And-Other-Sources.md)
  looks into other sources for OSM data than files
- [**Working With Change Files**](user_manual/08-Working-With-Change-Files.md)
  explores how to handle OSM diff files with updates
- [**Working with History Files**](user_manual/09-Working-With-History-Files.md)
  looks into the specifics of OSM files containing multiple versions of an object
- [**Replication Tools**](user_manual/10-Replication-Tools.md)
  lists the means how to obtain OSM update data with pyosmium

pyosmium builds on the fast and efficient [libosmium](https://osmcode.org/libosmium/)
library. It borrows many of its concepts from libosmium. For more in-depth
information, you might also want to consult the
[**libosmium manual**](https://osmcode.org/libosmium/manual.html).


---

## <a name="user_manual-01-first-steps-md"></a>File: user_manual\01-First-Steps.md

# First Steps

pyosmium is a library that processes data as a stream: it reads the data from
a file or other input source and presents the data to the user one object at
the time. This means that it can efficiently process large files with many
objects. The down-side is that it is not possible to directly access specific
objects as you need them. Instead it is necessary to apply some simple
techniques of caching and repeated reading of files to get all the data you
need. This takes some getting used to at the beginning but pyosmium gives
you the necessary tools to make it easy.

## File processing

pyosmium allows to process OSM files just like any other file: Open the
file by instantiating a [FileProcessor][osmium.FileProcessor], then iterate
over each OSM object in the file with a simple 'for' loop.

Lets start with a very simple script that lists the contents of file:


!!! example
    === "Code"
    ```python
    import osmium

    for obj in osmium.FileProcessor('buildings.opl'):
        print(obj)
    ```

    === "Output"
    ```
    n1: location=45.0000000/13.0000000 tags={}
    n2: location=45.0001000/13.0000000 tags={}
    n3: location=45.0001000/13.0001000 tags={}
    n4: location=45.0000000/13.0001000 tags={entrance=yes}
    n11: location=45.0000000/13.0000000 tags={}
    n12: location=45.0000500/13.0000000 tags={}
    n13: location=45.0000500/13.0000500 tags={}
    n14: location=45.0000000/13.0000500 tags={}
    w1: nodes=[1,2,3,4,1] tags={}
    w2: nodes=[11,12,13,14,11] tags={}
    r1: members=[w1,w2], tags={type=multipolygon,building=yes}
    ```

While iterating over the file, pyosmium decodes the data from the file
in the background and puts it into a buffer. It then returns a
_read-only view_ of each OSM object to Python. This is important to always
keep in mind. pyosmium never shows you a full data object, it only ever
presents a view. That means you can read and process the information about
the object but you cannot change it or keep the object around for later. Once
you retrieve the next object, the view will no longer be valid.

To show you what happens, when you try to keep the objects around, let us
slightly modify the example above. Say you want to have a more compact output
and just print for each object type, which IDs appear in the file. You might
be tempted to just save the object and create the formatted output only after
reading the file is done:

!!! bug "Buggy Example"
    === "Code"
    ```python
    # saves object by their type, more about types later
    objects = {'n' : [], 'w': [], 'r': []}

    for obj in osmium.FileProcessor('buildings.opl'):
        objects[obj.type_str()].append(obj)

    for otype, olist in objects.items():
        print(f"{otype}: {','.join(o.id for o in olist)}")
    ```

    === "Output"
    ```
    Traceback (most recent call last):
      File "bad_ref.py", line 10, in <module>
        print(f"{otype}: {','.join(o.id for o in olist)}")
                          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "bad_ref.py", line 10, in <genexpr>
        print(f"{otype}: {','.join(o.id for o in olist)}")
                                   ^^^^
      File "osmium/osm/types.py", line 313, in id
        return self._pyosmium_data.id()
               ^^^^^^^^^^^^^^^^^^^^^^^^
    RuntimeError: Illegal access to removed OSM object
    ```

As you can see, the code throws a runtime error complaining about an
'illegal access'. The `objects` dictionary doesn't contain any OSM objects.
It just has collected all the views on the objects. By the time the view
is accessed in the print function, the buffer the view points to is long gone.
pyosmium has invalidated the view. In practise this means that you need to
make an explicit copy of all information you need outside the loop iteration.

The code above can be easily "fixed" by saving only the id instead of the full
object. This also happens to be much more memory efficient:

!!! example
    === "Code"
    ```python
    objects = {'n' : [], 'w': [], 'r': []}

    for obj in osmium.FileProcessor('buildings.opl'):
        objects[obj.type_str()].append(obj.id)

    for otype, olist in objects.items():
        print(f"{otype}: {','.join(str(id) for id in olist)}")
    ```
    === "Output"
    ```
    n: 1,2,3,4,11,12,13,14
    w: 1,2
    r: 1
    ```

The output shows IDs for three different kind of objects: nodes, ways and
relations. Before we can continue, you need to understand the basics about
these types of objects and of the OpenStreetMap data model. If you are already
familiar with the structure of OSM data, you can go directly to the next
chapter.

## OSM data model

OpenStreetMap data is organised as a topological model. Objects are not
described with geometries like most GIS models do. Instead the objects are
described in how they relate to points in the world. This makes a huge
difference in how the data is processed.

An OSM object does not have a pre-defined function. What an object represents
is described with a set of properties, the _tags_. This is a simple key-value
store of strings. The meaning of the tags is not part of the data model
definition. Except for some minor technical limits, for example a maximum
length, any string can appear in the key and value of the tags. What keys and
values are used is decided through _consensus between users_. This gives OSM
a great flexibility to experiment with new kinds of data and evolve its
dataset. Over time a large set of agreed-upon tags has emerged for most kinds
of objects. These are the tags you will usually work with. You can search the
documentation in the [OSM Wiki](https://wiki.openstreetmap.org/) to find
out about the tags. It is also always useful to consult
[Taginfo](https://taginfo.openstreetmap.org/), which shows statistics over the
different keys and value in actual use.

Tags are common to all OSM objects. After that there are three kinds of
objects in OSM: nodes, ways and relations.

### Nodes

A node is a point on the surface of the earth. Its location is described through
its latitude and longitude using projection WSG84.

### Ways

Ways are lines that are created by connecting a sequence of nodes. The
nodes are described with the ID of a node in the database. That means
that a way object does not directly have coordinates. To find out about
the coordinates where the way is located, it is necessary to look up the
nodes of the way in the database and get their coordinates.

Representing a way through nodes has another interesting side effect:
many of the nodes in OSM are not meaningful in itself. They don't represent
a bus stop or lamp post or entrance or any other point of interest. They
only exist as supporting points for the ways and don't have any tags.

When a way ends at the same node ID where it starts, then the way may be
interpreted as representing an area. If it is really an area or just a linear
feature that happens to circle back on itself (for example, a fence around a
garden) depends on the tags of the way. Areas are handled more
in-depth in the chapter
[Working with Geometries](03-Working-with-Geometries.md#areas).

### Relations

A relation is an ordered collection of objects. Nodes, ways and relations all
can be a member in a relation. In addition, a relation member can be assigned
a _role_, a string that describes the function of a member. The data model
doesn't define what relations should be used for or how the members should
be interpreted.

## Forward and backward references

The topologic nature of the OSM data model means that an OSM object rarely
can be regarded in isolation. OSM ways are not meaningful without the
location information contained in its nodes. And conversely, changing the
location in a way also changes the geometry of the way even though the way
itself is not changed. This is an important concept to keep in mind when
working with OSM data. In this manual, we will use the terms forward and
backward references when talking about the dependencies between objects:

* A __forward reference__ means that an object is referenced to by another.
  Nodes appear in ways. Ways appear in relations. And a node may even have
  an indirect forward reference to a relation through a way it appear in.
  Forward references are important when tracking changes. When the location
  of a node changes, then all its forward references have to be reevaluated.

* A __backward reference__ goes from an object to its referenced children.
  Going from a way to its containing nodes means following a backward
  reference. Backward references are needed to get the complete geometry of
  an object: given that only nodes contain location information, we have
  to follow the backward references for ways and relations until we reach
  the nodes.

## Order in OSM files

OSM files usually follow a sorting convention to make life easier for
processing software: first come nodes, then ways, then relations. Each
group of objects is ordered by ID. One of the advantages of this order
is that you can be sure that you have been already presented with all
backward references to an object, when it appears in the processing loop.
Knowing this fact can help you optimise how often you have to read through
the file and speed up processing.

Sadly, there is an exception to the rule which is nested relations:
relations can of course contain other relations with a higher ID. If you
have to work with nested relations, rescanning the file multiple times
or keeping large parts of the file in memory is pretty much always unavoidable.


---

## <a name="user_manual-02-extracting-object-data-md"></a>File: user_manual\02-Extracting-Object-Data.md

# OSM Objects

This chapter explains more about the different object types that are
returned in pyosmium and how to access its data.

## Determining the Type of Object

pyosmium may return five different types of objects. First there are the
three base types from the OSM data model already
[introduced in the last chapter][osm-data-model]:
nodes, ways and relations. Next there is an _area type_. It is explained
in more detail in the [Geometry chapter](03-Working-with-Geometries.md#areas).
Finally, there is a type for changesets, which contains information about
edits in the OSM database. It can only appear in special changeset files
and explained in more detail [below](#changeset).

The FileProcessor may return any of these objects, when iterating over a file.
Therefore, a script will usually first need to determine the type of object
received. There are a couple of ways to do this.

#### Using `is_*()` convenience functions

All object types, except changesets, implement a set of
`is_node`/`is_way`/`is_relation`/`is_area` functions, which give a nicely
readable way of testing for a specific object type.

!!! example
    === "Code"
    ```python
    for o in osmium.FileProcessor('buildings.opl'):
        if o.is_relation():
            print('Found a relation.')
    ```
    === "Output"
    ```
    Found a relation.
    ```

#### Using the type identifier

The `type_str()` function returns the type of the object as a single
lower case character. The supported types are:

| Character | Type      |
|-----------|-----------|
| __n__     | node      |
| __w__     | way       |
| __r__     | relation  |
| __a__     | area      |
| __c__     | changeset |

This type string can be useful for printing or when saving data by
type. It can also be used to test for a specific type. It is particularly
useful when testing for multiple types:

!!! example
    === "Code"
    ```python
    for o in osmium.FileProcessor('../data/buildings.opl'):
        if o.type_str() in 'wr':
            print('Found a way or relation.')
    ```
    === "Output"
    ```
    Found a way or relation.
    Found a way or relation.
    Found a way or relation.
    ```

#### Testing for object type

Each OSM object type has a [corresponding Python class](../reference/Dataclasses.md).
You can simply test for this object type:

!!! example
    ```python
    for o in osmium.FileProcessor('buildings.opl'):
        if isinstance(o, osmium.osm.Relation):
            print('Found a relation.')
    ```


## Reading object tags

Every object has a list of properties, the tags. They can be accessed through
the `tags` property, which provides a simple dictionary-like view of the tags.
You can use the bracket notation to access a specific tag or use the more
explicit `get()` function. Just like for Python dictionaries, an access by
bracket raises a `ValueError` when the key you are looking for does not exist,
while the `get()` function returns the selected default value.

The `in` operation can be used to check for existence of a key:

!!! example
    ```python
    for o in osmium.FileProcessor('buildings.opl'):
        # When using the bracket notation, make sure the tag exists.
        if 'entrance' in o.tags:
            print('entrace =', o.tags['entrance'])

        # The get() function never throws.
        print('building =', o.tags.get('building', '<unset>')
    ```

Tags can also be iterated over. The iterator returns [Tag][osmium.osm.Tag]
objects. These each hold a key (`k`) and a value (`v`) string. A tag is
itself a Python iterable, so that you can easily iterate through keys and
values like this:

!!! example
    ```python
    from collections import Counter

    stats = Counter()

    for o in osmium.FileProcessor('buildings.opl'):
        for k, v in o.tags:
            stats.update([(k, v)])

    print("Most common tags:", stats.most_common(3))
    ```

As with all data in OSM objects, the tags property is only a view on tags
of the object. If you want to save the tag list for later use, you must make
a copy of the list. The most simple way to do this, is to convert the tag
list into a Python dictionary:

!!! example
    ```python
    saved_tags = []

    for o in osmium.FileProcessor('../data/buildings.opl'):
        if o.tags:
            saved_tags.append(dict(o.tags))

    print("Saved tags:", saved_tags)
    ```


## Other common meta information

Next to the tags, every OSM object also carries some meta information
describing its ID, version and information regarding the editor.

## Properties of OSM object types

### Nodes

The main property of a [Node][osmium.osm.Node] is the _location_,
a coordinate in WGS84 projection.
Latitude and longitude of the node can be accessed either through the
`location` property or through the `lat` and `lon` shortcuts:

!!! example
    ```python
    for o in osmium.FileProcessor('../data/buildings.opl', osmium.osm.NODE):
        assert (o.location.lon, o.location.lat) == (o.lon, o.lat)
    ```

OpenStreetMap, and by extension pyosmium, saves latitude and longitude
internally as a 7-digit fixed-point number. You can access the coordinates
as fixed-point integers through the `x` and `y` properties. There may be rare
use cases, where using this fixed-point notation is faster and more precise.

The coordinates returned by the `lat`/`lon` accessors are guaranteed to be
valid. That means that a value is set and is between -180 and 180 degrees
for longitude and -90 and 90 degrees for latitude. If the file contains
an invalid coordinate, then pyosmium will throw a `ValueError`. To access
the raw unchecked coordinates, use the
functions `location.lat_without_check()` and `location.lon_without_check()`.

### Ways

A [Way][osmium.osm.Way] is essentially an ordered sequence of nodes. This
sequence can be accessed through the `nodes` property. An OSM way only
stores the ID of each node. This can be rather inconvenient when you want
to work with the geometry of the way, because the coordinates of each
node need to be looked up. pyosmium therefore exposes a list of
[NodeRefs][osmium.osm.NodeRef] with the nodes property. Each element in this
list contains the node ID and optionally the location of the node. The
next chapter [Working with Geometries](03-Working-with-Geometries.md)
explains in detail, how pyosmium can help to fill the location of the node.

### Relations

A [Relation][osmium.osm.Relation] is also an ordered sequence. Each sequence
element can reference an arbitrary OSM object. In addition, each of the
members can be assigned a _role_, an arbitrary string that describes the
function of the member. The OSM data model does not specify what the function
of a member is and which roles are defined. You need to know what kind
of relation you are dealing with in order to understand what the members
are suppose to represent. Over the years, the OSM community has established
a convention that every relation comes with a `type` tag, which defines
the basic kind of the relation. For each type you can refer to the
Wiki documentation to learn about the meaning of members and roles.
The most important types currently in use are:

* [multipolygon](https://wiki.openstreetmap.org/wiki/Relation:multipolygon)
  describes an area geometry. Pyosmium natively supports creating geometries
  from this type of relation. See
  [Working with Geometries](03-Working-with-Geometries.md) for more information.
* [boundary](https://wiki.openstreetmap.org/wiki/Relation:boundary) 
  is a special form of the multipolygon type. It is used specifically for
  the various forms of boundaries and define some special roles
  for associated node objects.
* [route](https://wiki.openstreetmap.org/wiki/Relation:route) is for
  collections of ways that make up marked routes for hiking, cycling
  and other forms of transport.
* [public_transport](https://wiki.openstreetmap.org/wiki/Relation:public_transport)
  are a special form of the route relation made for routes of public transport
  vehicles (trains, buses, trams etc). They add some special member roles
  for the stops of the vehicles.
* [restriction](https://wiki.openstreetmap.org/wiki/Relation%3Arestriction)
  is for street-level routing and describes turn restrictions for vehicles.
* [associatedStreet](https://wiki.openstreetmap.org/wiki/Relation:associatedStreet)
  relation types are used in some parts of the world to create a connection
  between address points and the street they belong to.

The members of a relation can be accessed through the `members` property.
This is a simple list of [RelationMember][osmium.osm.RelationMember] objects.
They expose the OSM type of the member, its ID and a role string. When no
role has been set, the `role` property returns an empty string. Here is an
example of a simple iteration over all members:

!!! example
    ```python
    for o in osmium.FileProcessor('buildings.opl', osmium.osm.RELATION):
        for member in o.members:
            print(f"Type: {member.type}  ID: {member.ref}  Role: {member.role}")
    ```


The member property provides only a temporary read-only view of the members.
If you want to save the list for later processing, you need to make an explicit
copy like this:

!!! example
    ```python
    memberlist = {}

    for o in osmium.FileProcessor('buildings.opl', osmium.osm.RELATION):
        memberlist[o.id] = [(m.type, m.ref, m.role) for m in o.members]

    print(memberlist)
    ```


Always keep in mind that relations can become very large. Some have thousands
of members. Therefore consider very carefully which members you are actually
interested when saving members and only keep those that are actually needed later.

### Changeset

The [Changeset][osmium.osm.Changeset] type is the odd one out among the
OSM data types. It does not contain actual map data. Instead it is use
to save meta information about the edits made to the OSM database. You
normally don't find Changeset objects in a datafile. Changeset information
is published in separate files.


---

## <a name="user_manual-03-working-with-geometries-md"></a>File: user_manual\03-Working-with-Geometries.md

# Working with Geometries

When working with map data, sooner or later, you will need the geometry
of an object: a point, a line or a polygon. OSM's topologic data model
doesn't make them directly available with each object. In order to build
a geometry for an object, the location information from referenced nodes
need to be collected and then the geometry can be assembled from that.
pyosmium provides a number of data structures and helpers to create
geometries for OSM objects.

## Geometry types

### Point geometries

OSM nodes are the only kind of OSM object that produce a point geometry.
The location of the point is directly stored with the OSM nodes. This
makes it straightforward to extract such a geometry:

!!! example
    === "Code"
    ```python
    for o in osmium.FileProcessor('buildings.opl', osmium.osm.NODE):
        print(f"Node {o.id}: lat = {o.lat} lon = {o.lon}")
    ```
    === "Output"
    ```
    Node 1: lat = 13.0 lon = 45.0
    Node 2: lat = 13.0 lon = 45.0001
    Node 3: lat = 13.0001 lon = 45.0001
    Node 4: lat = 13.0001 lon = 45.0
    Node 11: lat = 13.00001 lon = 45.00001
    Node 12: lat = 13.00001 lon = 45.00005
    Node 13: lat = 13.00005 lon = 45.00005
    Node 14: lat = 13.00005 lon = 45.00001
    ```

### Line geometries

Line geometries are usually created from OSM ways. The OSM way object does
not contain the coordinates of a line geometry directly. It only contains a
list of references to OSM nodes. To create a line geometry from an OSM way,
it is necessary to look up the coordinate of each referenced node.
pyosmium provides an efficient way to do so: the location storage. The storage
automatically records the coordinates of each node that is read from the file
and caches them for future use. When later a way is read from a file, the
list of nodes in the way is augmented with the appropriate coordinates.
Location storage is not enabled by default. To add it to the processing,
use the function [`with_locations()`][osmium.FileProcessor.with_locations]
of the FileProcessor.

!!! example
    === "Code"
    ```python
    for o in osmium.FileProcessor('../data/buildings.opl').with_locations():
        if o.is_way():
            coords = ", ".join((f"{n.lon} {n.lat}" for n in o.nodes if n.location.valid()))
            print(f"Way {o.id}: LINESTRING({coords})")
    ```
    === "Output"
    ```
    Way 1: LINESTRING(45.0 13.0, 45.0001 13.0, 45.0001 13.0001, 45.0 13.0001, 45.0 13.0)
    Way 2: LINESTRING(45.00001 13.00001, 45.00005 13.00001, 45.00005 13.00005, 45.00001 13.00005, 45.00001 13.00001)
    ```


Not all OSM files are _reference-complete_. It can happen that some nodes
which are referenced by a way are missing from a file. Always write your
code so that it can work with incomplete geometries. In particular, you
should be aware that there is no guarantee that an OSM way will translate
into a valid line geometry. An OSM way may consist of only one node.
Or two subsequent coordinates in the line are exactly at the same position.

pyosmium provides different implementations for the location storage. The
default should be suitable for small to medium-sized OSM files. See the
paragraph on [Location storage][location-storage] below for more information
on the different types of storages and how to switch them.

### Areas

OSM has two different ways to model area geometries: they may be derived
from way objects or relation objects.

A way can be interpreted as an area when it is _closed_. That happens when
the first and the last node are exactly the same. You can use the function
[`is_closed()`][osmium.osm.Way.is_closed].

Not every closed way necessarily represents and area. Think of a little
garden with a fence around it. If the OSM way represents the garden, then it
should be interpreted as an area. If it represents the fence, then it is a
line geometry that just happens to go full circle. You need to look at the
tags of a way in order to decide if it should become an area or a line,
or sometimes even both.

There are two types of relations that also represent areas. If the relation
is tagged with `type=multipolygon` or `type=boundary` then it is by
convention an area independently of all the other tags of the relation.

pyosmium implements a special handler for the processing of areas. This
handler creates a new type of object, the [Area][osmium.osm.Area] object,
and makes it available like the other OSM types. It can be enabled
with the [`with_areas()`][osmium.FileProcessor.with_areas] function:

!!! example
    === "Code"
    ```python
    objects = ''
    areas = ''
    for o in osmium.FileProcessor('../data/buildings.opl').with_areas():
        objects += f" {o.type_str()}{o.id}"
        if o.is_area():
            areas += f" {o.type_str()}{o.id}({'w' if o.from_way() else 'r'}{o.orig_id()})"

    print("OSM objects in this file:", objects)
    print("Areas in this file:", areas)
    ```
    === "Output"
    ```
    OSM objects in this file:  n1 n2 n3 n4 n11 n12 n13 n14 w1 w2 r1 a2 a3
    Areas in this file:  a2(w1) a3(r1)
    ```

Note how Area objects are added to the iterator _in addition_ to the original
OSM data. During the processing of the loop, there is first OSM way 1 and
then the Area object 2, which corresponds to the same way.

When the area handler is enabled, the FileProcessor scans the file twice:
during the first run information about all relations that might be areas
is collected. This information is then used in the main run of the file
processor, where the areas are assembled as soon as all the necessary
objects that are part of each relation have been collected.

The area handler automatically enables a location storage because it needs
access to the node geometries. It will set up the default implementation.
To use a different implementation, simply use `with_locations()` with
a custom storage together with `with_areas()`.

#### The pyosmium Area type

The Area type has the same common attributes as the other OSM types.
However, it produces its own special ID space. This is necessary because
an area might be originally derived from a relation or way. When derived
from a way, the ID is computed as `2 * way ID`. When it is derived from
a relation, the ID is `2 * relation ID + 1`. Use the function
[`from_way()`][osmium.osm.Area.from_way] to check what type the original OSM
object is and the function [`orig_id()`][osmium.osm.Area.orig_id]
to get the ID of the underlying object.

The polygon information is organised in lists of rings. Use
[`outer_rings()`][osmium.osm.Area.outer_rings] to iterate over the rings
of the polygon that form outer boundaries of the polygon. The data structures
for these rings are node lists just like the ones used in OSM ways.
They always form a closed line that goes clockwise. Each outer ring can have
one or more holes. These can be iterated through with the
[`inner_rings()`][osmium.osm.Area.inner_rings] function. The inner rings
are also a node list but will go anti-clockwise. To illustrate how to process
the functions, here is the simplified code to create the
[WKT](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry)
representation of the polygon:

!!! example
    === "Code"
    ```python
    for o in osmium.FileProcessor('../data/buildings.opl').with_areas():
        if o.is_area():
            polygons = []
            for outer in o.outer_rings():
                rings = "(" + ", ".join((f"{n.lon} {n.lat}" for n in outer if n.location.valid())) + ")"
                for inner in o.inner_rings(outer):
                    rings += ", (" + ", ".join((f"{n.lon} {n.lat}" for n in outer if n.location.valid())) + ")"
                polygons.append(rings)
            if o.is_multipolygon():
                wkt = f"MULTIPOLYGON(({'), ('.join(polygons)}))"
            else:
                wkt = f"POLYGON({polygons[0]})"
            print(f"Area {o.id}: {wkt}")        
    ```
    === "Output"
    ```
    Area 2: POLYGON((45.0 13.0, 45.0001 13.0, 45.0001 13.0001, 45.0 13.0001, 45.0 13.0))
    Area 3: POLYGON((45.0 13.0, 45.0001 13.0, 45.0001 13.0001, 45.0 13.0001, 45.0 13.0), (45.0 13.0, 45.0001 13.0, 45.0001 13.0001, 45.0 13.0001, 45.0 13.0))
    ```


### Geometries from other relation types

OSM has many other relation types apart from the area types. pyosmium has
no special support for other relation types yet. You need to manually
assemble geometries by collecting the geometries of the members.

## Geometry Factories

pyosmium has a number of geometry factories to make it easier to convert
an OSM object to well known geometry formats. To use them, instantiate
the factory once and then hand in the OSM object to one of the create
functions. A code snippet that converts all objects into WKT format looks
approximately like that:

!!! example
    ```python
    fab = osmium.geom.WKTFactory()

    for o in osmium.FileProcessor('../data/buildings.opl').with_areas():
        if o.is_node():
            wkt = fab.create_point(o.location)
        elif o.is_way() and not o.is_closed():
            wkt = fab.create_linestring(o.nodes)
        elif o.is_area():
            wkt = fab.create_multipolygon(o)
        else:
            wkt = None # ignore relations
    ```

There are factories for GeoJSON ([`osmium.geom.GeoJSONFactory`][]),
well-known text ([`osmium.geom.WKTFactory`][])
and well-known binary ([`osmium.geom.WKBFactory`][]) formats.

## Python Geo Interface

If you want to process the geometries with Python libraries like
[shapely](https://shapely.readthedocs.io)[^1] or [GeoPandas](https://geopandas.org),
then the standardized [__geo_interface__](https://gist.github.com/2217756)
format can come in handy.

[^1]: Shapely only received full support for geo_interface geometries with
      features in version 2.1. For older versions create WKT geometries as
      explained above and create Shapely geometries from that.

pyosmium has a special filter [GeoInterfaceFilter][osmium.filter.GeoInterfaceFilter]
which enhances pyosmium objects with a `geo_interface` attribute.
This allows libraries that support this interface to directly consume the
OSM objects. The GeoInterfaceFilter needs location information to create
the geometries. Don't forget to add `with_locations()` and/or
`with_areas()` to the FileProcessor.

Here is an example that computes the total length of highways using
the geometry functions of shapely:

!!! example
    === "Code"
    ```python
    from shapely.geometry import shape

    fp = osmium.FileProcessor('liechtenstein.osm.pbf')\
               .with_locations().\
               with_filter(osmium.filter.GeoInterfaceFilter())

    total = 0.0
    for o in fp:
        if o.is_way() and 'highway' in o.tags:
            # Shapely has only support for Features starting from version 2.1,
            # so lets cheat a bit here.
            geom = shape(o.__geo_interface__['geometry'])
            # Length is computed in WGS84 projection, which is practically meaningless.
            # Lets pretend we didn't notice, it is an example after all.
            total += geom.length

    print("Total length:", total)
    ```
    === "Output"
    ```
    Total length: 14.58228287312081
    ```


For an example on how to use the Python Geo Interface together with GeoPandas,
have a look at the [Visualisation Recipe](../cookbooks/Visualizing-Data-With-Geopandas.ipynb).

## Location Storage

See the [Osmium manual](https://osmcode.org/osmium-concepts/#indexes)
for the different types of location storage.


---

## <a name="user_manual-04-working-with-filters-md"></a>File: user_manual\04-Working-with-Filters.md

# Pre-Filtering Input Data

When processing an OSM file, it is often only a very small part of the
objects the script really needs to see and process. Say, you are interested
in the road network, then the millions of buildings in the file could easily
be skipped over. This is the task of filters. They provide a fast and
performance-efficient way to pre-process or skip over data before it is
processed within the Python code.

## How filters work

Filters can be added to a FileProcessor with the
[`with_filter()`][osmium.FileProcessor.with_filter] function. An arbitrary
number of filters can be added to the processor. Simply call the functions
as many times as needed. The filters will be executed in the order they
have been added. If any of the filters marks the object for removal,
the object is immediately dropped and the next object from the file is
processed.

Filters can have side effects. That means that a filter may add additional
attributes to the OSM object it processes and these attributes will
be visible for subsequent filters and in the Python processing code.
For example, the GeoInterfaceFilter adds a Python `__geo_interface__`
attribute to the object.

Filters can be restricted to process only certain types of OSM objects.
If an OSM object doesn't have the right type, the filter will be skipped
over as if it wasn't defined at all. To restrict the types, call the
[`enable_for()`][osmium.BaseFilter.enable_for] function.

Here is an example of a FileProcessor where only place nodes and
boundary ways and relations are iterated through:

!!! example
    ```python
    fp = osmium.FileProcessor('../data/liechtenstein.osm.pbf')\
               .with_filter(osmium.filter.KeyFilter('place').enable_for(osmium.osm.NODE))\
               .with_filter(osmium.filter.KeyFilter('boundary').enable_for(osmium.osm.WAY | osmium.osm.RELATION))
    ```

## Fallback Processing

Once an object has been filtered, the default behaviour of the FileProcessor
is to simply drop the object. Sometimes it can be useful to do something
different with the object. For example, when you want to change some tags
in a file and then write the data out again, then you'd usually want to filter
out the objects that are not to be modified. However, you wouldn't want
to drop them completely but write the unmodified object out. For such cases
it is possible to set a fallback handler for filtered objects using the
[`handler_for_filtered()`][osmium.FileProcessor.handler_for_filtered] function.

The file writer can become a fallback handler for the file processor. The
[next chapter Handlers](05-Working-with-Handlers.md) will show how to write
a custom handler that can be used in this function.


## Built-in Filters

The following section shortly describes the filters that are built into pyosmium.

### EmptyTagFilter

This filter removes all objects that have no tags at all. Most of the nodes
in an OSM files fall under this category. So even when you don't want to
apply any other filters, this one can make a huge difference in processing time:

!!! example
    === "Code"
    ```python
    print("Total number of objects:",
          sum(1 for o in osmium.FileProcessor('liechtenstein.osm.pbf')))

    print("Total number of tagged objects:",
          sum(1 for o in osmium.FileProcessor('liechtenstein.osm.pbf')
                               .with_filter(osmium.filter.EmptyTagFilter())))
    ```
    === Output
    ```
    Total number of objects: 340175
    Total number of tagged objects: 49645
    ```


### EntityFilter

The Entity filter only lets through objects of the selected type:


!!! example
    === "Code"
    ```python
    print("Total number of objects:",
          sum(1 for o in osmium.FileProcessor('../data/liechtenstein.osm.pbf')))

    print("Of which are nodes:",
          sum(1 for o in osmium.FileProcessor('../data/liechtenstein.osm.pbf')
                               .with_filter(osmium.filter.EntityFilter(osmium.osm.NODE))))
    ```
    === "Output"
    ```
    Total number of objects: 340175
    Of which are nodes: 306700
    ```


On the surface, the filter is very similar to the entity selector that
can be passed to the FileProcessor. In fact, it would be much faster to count
the nodes using the entity selector:

!!! example
    ```python
    print("Of which are nodes:",
          sum(1 for o in osmium.FileProcessor('../data/liechtenstein.osm.pbf', osmium.osm.NODE)))
    ```
    === "Output"
    ```
    Of which are nodes: 306700
    ```


However, the two implementations use different mechanism to drop the nodes.
When the entity selector in the FileProcessor is used like in the second
example, then only the selected entities are read from the file. In our
example, the file reader would skip over the ways and relations completely.
When the entity filter is used, then the entities are only dropped when
they get to the filter. Most importantly, the objects will still be visible
to any filters applied _before_ the entity filter.

This can become of some importance when working with geometries. Lets say
we can to compute the length of all highways in our file. You will remember
from the last chapter about [Working with Geometries](03-Working-with-Geometries.md)
that it is necessary to enable the location cache in order to be able to
get the geometries of the road:

!!! example
    === "Code"
    ```python
    total = 0.0

    for o in osmium.FileProcessor('../data/liechtenstein.osm.pbf')\
        .with_locations()\
        .with_filter(osmium.filter.EntityFilter(osmium.osm.WAY)):
        if 'highway' in o.tags:
            total += osmium.geom.haversine_distance(o.nodes)

    print(f'Total length of highways is {total/1000} km.')
    ```
    === "Output"
    ```
    Total length of highways is 1350.8030544343883 km.
    ```


The location cache needs to see all nodes in order to record their locations.
This would not happen if the file reader skips over the nodes. It is
therefore imperative to use the entity filter here. In fact, pyosmium will
refuse to run when nodes are not enabled in a FileProcessor with location
caching:

!!! bug "Bad example"
    === "Code"
    ```python
    for o in osmium.FileProcessor('../data/liechtenstein.osm.pbf', osmium.osm.WAY).with_locations():
        if 'highway' in o.tags:
            osmium.geom.haversine_distance(o.nodes)
    ```
    === "Output"
    ```
    ---------------------------------------------------------------------------

    RuntimeError                              Traceback (most recent call last)

    Cell In[14], line 1
    ----> 1 for o in osmium.FileProcessor('../data/liechtenstein.osm.pbf', osmium.osm.WAY).with_locations():
          2     if 'highway' in o.tags:
          3         osmium.geom.haversine_distance(o.nodes)


    File ~/osm/dev/pyosmium/build/lib.linux-x86_64-cpython-311/osmium/file_processor.py:46, in FileProcessor.with_locations(self, storage)
         42 """ Enable caching of node locations. This is necessary in order
         43     to get geometries for ways and relations.
         44 """
         45 if not (self._entities & osmium.osm.NODE):
    ---> 46     raise RuntimeError('Nodes not read from file. Cannot enable location cache.')
         47 if isinstance(storage, str):
         48     self._node_store = osmium.index.create_map(storage)


    RuntimeError: Nodes not read from file. Cannot enable location cache.
    ```


### KeyFilter

This filter only lets pass objects where its list of tags has any of the
keys given in the arguments of the filter.

If you want to ensure that all of the keys are present, use the
KeyFilter multiple times:

!!! example
    ```python
    print("Objects with 'building' _or_ 'amenity' key:",
          sum(1 for o in osmium.FileProcessor('../data/liechtenstein.osm.pbf')
                               .with_filter(osmium.filter.KeyFilter('building', 'amenity'))))

    print("Objects with 'building' _and_ 'amenity' key:",
          sum(1 for o in osmium.FileProcessor('../data/liechtenstein.osm.pbf')
                               .with_filter(osmium.filter.KeyFilter('building'))
                               .with_filter(osmium.filter.KeyFilter('amenity'))))
    ```

### TagFilter

This filter works like KeyFilter, allowing both AND and OR combinations, but
it requires whole tags (key and value) in the object's tag list.
Tags are given as two-element tuples.

!!! example
    ```python
    print("Objects with 'highway=primary' _or_ 'surface=asphalt' tags:",
          sum(1 for o in osmium.FileProcessor('../data/liechtenstein.osm.pbf')
                               .with_filter(osmium.filter.TagFilter(('highway','primary'), ('surface','asphalt')))))

    print("Objects with 'highway=primary' _and_ 'surface=asphalt' tags:",
          sum(1 for o in osmium.FileProcessor('../data/liechtenstein.osm.pbf')
                               .with_filter(osmium.filter.TagFilter(('highway','primary')))
                               .with_filter(osmium.filter.TagFilter(('surface','asphalt')))))
    ```

### IdFilter

This filter takes an iterable of numbers and lets only pass objects that
have an ID that matches the list. This filter is particularly useful when
doing a two-stage processing, where in the first stage the file is scanned
for objects that are of interest (for example, members of certain relations)
and then in the second stage these objects are read from the file. You
pretty much always want to use this filter in combination with the
`enable_for()` function to restrict it to a certain object type.

In its purest form, the filter could be used to search for a single object
in a file:

!!! example
```python
fp = osmium.FileProcessor('../data/buildings.opl')\
           .with_filter(osmium.filter.EntityFilter(osmium.osm.WAY))\
           .with_filter(osmium.filter.IdFilter([1]))

for o in fp:
    print(o)
```

However, in practise it is a very expensive way to find a single object.
Remember that the entire file will be scanned by the FileProcessor just
to find that one piece of information.

## Custom Python Filters

It is also possible to define a custom filter in Python. Most of the time
this is not very useful because calling a filter implemented in Python is
just as expensive as returning the OSM object to Python and doing the
processing then. However, it can be useful when the FileProcessor is
used as an Iterable input to other libraries like GeoPandas.

A Python filter needs to be implemented as a class that looks exactly
like a [Handler class](05-Working-with-Handlers.md): for each type that
should be handled by the filter, implement a callback function
`node()`, `way()`, `relation()`, `area()` or `changeset()`. If a callback
for a certain type is not implemented, then the object type will automatically
pass through the filter. The callback function needs to return either 'True',
when the object should be filtered out, or 'False' when it should pass through.

Here is a simple example of a filter that filters out all nodes that are older than 2020:

!!! example
    ```python
    import datetime as dt

    class DateFilter:

        def node(self, n):
            return n.timestamp < dt.datetime(2020, 1, 1, tzinfo=dt.UTC)


    print("Total number of objects:",
          sum(1 for o in osmium.FileProcessor('../data/liechtenstein.osm.pbf')))

    print("Without nodes older than 2020:",
          sum(1 for o in osmium.FileProcessor('../data/liechtenstein.osm.pbf')
                               .with_filter(DateFilter())))
    ```


---

## <a name="user_manual-05-working-with-handlers-md"></a>File: user_manual\05-Working-with-Handlers.md

# Handler-based Processing

All examples so far have used the FileProcessor for reading files. It provides
an iterative way of working through the data, which comes quite natural to
a Python programmer. This chapter shows a different way of processing a file.
It shows how to create one or more handler classes and apply those to
an input file.

_Note:_ handler classes used to be the only way of processing data in
older pyosimum versions. You may therefore find them in many tutorials
and examples. There is no disadvantage in using FileProcessors instead.
Handlers simply provide a different syntax for achieving a similar goal.

## The handler object and osmium.apply

A pyosmium handler object is simply a Python object that implements callbacks
to handle the different types of entities (`node`, `way`, `relation`, `area`,
`changeset`). Usually you would define a class with your handler functions
and instantiate it. A complete handler class that prints out each object
in the file would look like this:

!!! example
    ```python
    class PrintHandler:
        def node(self, n):
            print(n)

        def way(self, w):
            print(w)

        def relation(self, r):
            print(r)

        def area(self, a):
            print(a)

        def changeset(self, c):
            print(c)
    ```

Such a handler is applied to an OSM file with the function
[`osmium.apply()`][osmium.apply]. The function takes a single file as an
argument and then an arbitrary number of handlers:

!!! example
    === "Code"
    ```python
    import osmium

    my_handler = PrintHandler()

    osmium.apply('buildings.opl', my_handler)
    ```
    === "Output"
    ```
    n1: location=45.0000000/13.0000000 tags={}
    n2: location=45.0001000/13.0000000 tags={}
    n3: location=45.0001000/13.0001000 tags={}
    n4: location=45.0000000/13.0001000 tags={entrance=yes}
    n11: location=45.0000100/13.0000100 tags={}
    n12: location=45.0000500/13.0000100 tags={}
    n13: location=45.0000500/13.0000500 tags={}
    n14: location=45.0000100/13.0000500 tags={}
    w1: nodes=[1,2,3,4,1] tags={amenity=restaurant}
    w2: nodes=[11,12,13,14,11] tags={}
    r1: members=[w1,w2], tags={type=multipolygon,building=yes}
    ```

## Using filters with apply

[Filter functions](04-Working-with-Filters.md) are also recognised as handlers
by the apply functions. They have the same effect as when used in FileProcessors:
when they signal to filter out an object, then the processing is stopped for
that object and the next object is processed. You can arbitrarily mix filters
and custom-made handlers. They are sequentially executed in the order in which
they appear in the apply function:

!!! example
    ```python
    osmium.apply('buildings.opl',
                 osmium.filter.EntityFilter(osmium.osm.RELATION),
                 my_handler,
                 osmium.filter.KeyFilter('route')),
                 my_other_handler
    ```


## The `osmium.SimpleHandler` class

The `apply` function is a very low-level function for processing. It will
only apply the handler functions to the input and be done with it.
It will in particular not care about providing the necessary building blocks
for [geometry processing](03-Working-with-Geometries.md). If you need to
work with geometries, you can derive your handler class from 
[`osmium.SimpleHandler`][]. This mix-in class adds two convenience functions
to your handler : [`apply_file()`][osmium.SimpleHandler.apply_file]
and [`apply_buffer()`][osmium.SimpleHandler.apply_buffer].
These functions apply the handler itself to a file or buffer but come with
additional parameter to enable location. If the handler implements an `area`
callback, then they automatically enable area processing as well.


---

## <a name="user_manual-06-writing-data-md"></a>File: user_manual\06-Writing-Data.md

# Writing Data

pyosmium can also be used to write OSM files. It offers different writer
classes which support creating referentially correct files.

## Basic writer usage

All writers are created by instantiating them with the name of the file to
write to.

!!! example
    ```python
    writer = osmium.SimpleWriter('my_extra_data.osm.pbf')
    ```

The format of the output file is usually determined through the file prefix.
pyosmium will refuse to overwrite any existing files. Either make sure to
delete the files before instantiating a writer or use the parameter
`overwrite=true`.

Once a writer is instantiated, one of the `add*` functions can be used to
add an OSM object to the file. You can either use one of the
`add_node/way/relation` functions to force writing a specific type of
object or use the generic `add` function, which will try to determine the
object type. The OSM objects are directly written out in the order in which
they are given to the writer object. It is your responsibility as a user to
make sure that the order is correct with respect to the
[conventions for object order][order-in-osm-files].

After writing all data the writer needs to be closed using the `close()`
function. It is usually easier to use a writer as a context manager.

Here is a complete example for a script that converts a file from OPL format
to PBF format:

!!! example
    ```python
    with osmium.SimpleWriter('buildings.osm.pbf') as  writer:
        for o in osmium.FileProcessor('buildings.opl'):
            writer.add(o)
    ```

### Writing modified objects

In the example above an OSM object from an input file was written out directly
without modifications. Writers can accept OSM nodes, ways and relations
that way. However, usually you want to modify some of the data in the object
before writing it out again. Use the `replace()` function to create a
_mutable version_ of the object with the given parameters replaced.

Say you want to create a copy of a OSM file with all `source` tags removed:

!!! example
    ```python
    with osmium.SimpleWriter('buildings.osm.pbf') as  writer:
        for o in osmium.FileProcessor('buildings.opl'):
            if 'source' in tags:
                new_tags = dict(o.tags) # make a copy of the tags
                del new_tags['source']
                writer.add(o.replace(tags=new_tags))
            else:
                # No source tag. Write object out as-is.
                writer.add(o)
    ```

### Writing custom objects

You can also write data that is not based on OSM input data at all. The write
functions will accept any Python object that mimics the attributes of a
node, way or relation.

Here is a simple example that writes out four random points:

!!! example
    ``` python
    from random import uniform

    class RandomNode:
        def __init__(self, name, id):
            self.id = id
            self.location = (uniform(-180, 180), uniform(-90, 90))
            self.tags = {'name': name}

    with osmium.SimpleWriter('points.opl') as writer:
        for i in range(4):
            writer.add_node(RandomNode(f"Random {i}", i))
    ```

The following table gives an overview over the recognised attributes and
acceptable types. If an attribute is missing, then pyosmium will choose a
suitable default or leave the attribute out completely from the output if
that is possible.

| attribute | types |
|-----------|----------------------------|
| id        | `int` |
| version   | `int` (positive non-zero value) |
| visible   | `bool` |
| changeset | `int` (positive non-zero value) |
| timestamp | `str` or `datetime` (will be translated to UTC first) |
| uid       | `int` |
| tags      | [osmium.osm.TagList][], a dict-like object or a list of tuples, where each tuple contains a (key, value) string pair |
| user      | `str` |
| location  | _(node only)_ [osmium.osm.Location][] or a tuple of lon/lat coordinates |
| nodes     | _(way only)_ [osmium.osm.NodeRefList][] or a list consisting of either [osmium.osm.NodeRef][]s or simple node ids |
| members   | _(relation only)_ [osmium.osm.RelationMemberList][] or a list consisting of either [osmium.osm.RelationMember][]s or tuples of `(type, id, role)`. The member type must be a single character 'n', 'w' or 'r'. |

The `osmium.osm.mutable` module offers pure Python-object versions of `Node`,
`Way` and `Relation` to make the creation of custom objects easier. Any of
the allowable attributes may be set in the constructor. This makes the
example for writing random points a bit shorter:

!!! example
    ``` python
    from random import uniform

    with osmium.SimpleWriter('points.opl') as writer:
        for i in range(4):
            writer.add_node(osmium.osm.mutable.Node(
                id=i, location = (uniform(-180, 180), uniform(-90, 90)),
                tags={'name': f"Random {i}"}))
    ```


## Writer types

pyosmium implements three different writer classes: the basic
[SimpleWriter][osmium.SimpleWriter] and
the two reference-completing writers
[ForwardReferenceWriter][osmium.ForwardReferenceWriter] and
[BackReferenceWriter][osmium.BackReferenceWriter].


---

## <a name="user_manual-07-input-formats-and-other-sources-md"></a>File: user_manual\07-Input-Formats-And-Other-Sources.md

# Input Formats and Other Sources

pyosmium can read OSM data from different sources and in different formats.

## Supported file formats

pyosmium has built-in support for the most common OSM data formats as well
as formats specific to libosmium. The format to use is usually determined by the
suffix of the file name. The following table gives an overview over the
suffix recognised, the corresponding format and if the formats support reading
and/or writing.

| Suffix         | Reading | Writing | Format |
|----------------|---------|---------|--------|
| `.pbf`         | :material-check: | :material-check: | protobuf-based PBF format |
| `.osm`         | :material-check: {: rowspan=2} | :material-check: {: rowspan=2} | Original XML format {: rowspan=2} |
| `.xml`         | &#8288 {: style="padding:0"}| &#8288 {: style="padding:0"}| &#8288 {: style="padding:0"} |
| `.o5m`         | :material-check: | :material-close: | Custom format created by the [osmc tools](https://wiki.openstreetmap.org/wiki/Osmfilter) |
| `.opl`         | :material-check: | :material-check: | Osmium's line based text format [OPL](https://osmcode.org/opl-file-format/) |
| `.debug`       | :material-close: | :material-check: | a verbose human-readable format |
| `.ids`         | :material-close: | :material-check: | Text format only containing the object IDs |

All formats also support compression with gzip (suffix `.gz`)
and bzip2 (suffix `.bz2`) with the exception of the PBF format.

The suffixes may be further prefixed by three subtypes:

* __`.osm`__ - A simple OSM data file.
  Each object appears at most with one version in the file.
* __`.osc`__ - An [OSM change file](08-Working-With-Change-Files.md).
  Multiple versions of an object may appear in the file. The file is
  usually not reference-complete.
* __`.osh`__ - An [OSM history file](09-Working-With-History-Files.md).
  Objects appear with all available versions in the file. The file is
  usually reference-complete.

Thus the type `.osh.xml.bz2` would be an OSM history file in XML format
that has been compressed using the bzip2 algorithm.

If you have file inputs where the suffix differs from the internal format,
the file type can be explicitly set by instantiating an [osmium.io.File][]
object. It takes an optional format parameter which then must contain
the suffix notation of the desired file format.

!!! example
    This example forces the given input text file to be read as OPL.

    ```python
    fp = osmium.FileProcessor(osmium.io.File('assorted.txt', 'opl'))
    ```

## Using standard input and output

The special file name `-` can be used to read from standard input or
write to standard output.

When reading data, use a `File` object to specify the file format. With
the SimpleReader, you need to use the parameter `filetype`.

!!! example
    This code snipped dumps all ids of your input file to the console.

    ```python
    with osmium.SimpleWriter('-', filetype='ids') as writer:
        for o in osmium.FileProcessor('test.pbf'):
            writer.add(o)
    ```

## Reading from buffers

pyosmium can also read data from a in-memory byte buffer. Simply wrap the
buffer in a [osmium.io.FileBuffer][]. The file format always needs to be
explicitly given.

!!! example
    Reading from a buffer comes in handy when loading OSM data from a URL.
    This example computes statistics over data downloaded from an URL.

    ```python
    import urllib.request as urlrequest

    data = urlrequest.urlopen('https://example.com/some.osm.gz').read()

    counter = {'n': 0, 'w': 0, 'r': 0}

    for o in osmium.FileProcessor(osmium.io.FileBuffer(data, 'osm.gz')):
        counter[o.type_str()] += 1

    print("Nodes: %d" % counter['n'])
    print("Ways: %d" % counter['w'])
    print("Relations: %d" % counter['r'])
    ```


---

## <a name="user_manual-08-working-with-change-files-md"></a>File: user_manual\08-Working-With-Change-Files.md

# Working With Change Files

OpenStreetMap produces two kinds of data, full data files and diff files with updates. This chapter explains how to handle diff files.


---

## <a name="user_manual-09-working-with-history-files-md"></a>File: user_manual\09-Working-With-History-Files.md

# Working With History Files

An OSM data file usually contains data of a snapshot of the OpenStreetMap database at a certain point in time. The full database contains even more data. It has all the changes that were ever made. The full version of the database with the complete history is contained in so called history files. They do require some special attention when processing.


---

## <a name="user_manual-10-replication-tools-md"></a>File: user_manual\10-Replication-Tools.md

# Replication Tools

OpenStreetMap is a database that is constantly extended and updated. When you
download the planet or an extract of it, you only get a snapshot of the
database at a given point in time. To keep up-to-date with the development
of OSM, you either need to download a new snapshot or you can update your
existing data from change files published along with the planet file.
Pyosmium ships with two tools that help you to process change files:
`pyosmium-get-changes` and `pyosmium-up-to-date`.

This section explains the basics of OSM change files and how to use Pyosmium's
tools to keep your data up to date.

## About change files

Regular [change files](https://wiki.openstreetmap.org/wiki/Planet.osm/diffs)
are published for the planet and also by some extract services. These
change files are special OSM data files containing all changes to the database
in a regular interval. Change files are not referentially complete. That means
that they only contain OSM objects that have changed but not necessarily
all the objects that are referenced by the changed objects. Because of
that change file are rarely useful on their own. But they can be used
to update an existing snapshot of OSM data.

## Getting change files

There are multiple sources for OSM change files available:

 * [https://planet.openstreetmap.org/replication](https://planet.openstreetmap.org/replication)
   is the official source
   for planet-wide updates. There are change files for
   minutely, hourly and daily intervals available.

 * [Geofabrik](https://download.geofabrik.de) offers daily change files
   for all its updates. See the extract page for a link to the replication URL.
   Note that change files go only about 3 months back. Older files are deleted.

 * download.openstreetmap.fr offers
   [minutely change files](https://download.openstreetmap.fr/replication/)
   for all its [extracts](https://download.openstreetmap.fr/extracts/).

For other services also check out the list of providers on the
[OSM wiki](https://wiki.openstreetmap.org/wiki/Planet.osm).

## Updating a planet or extract

If you have downloaded the full planet or obtain a PBF extract file from one of the
sites which offer a replication service, then updating your OSM file can be as easy as:

    pyosmium-up-to-date <osmfile.osm.pbf>

This finds the right replication source and file to start with, downloads
changes and updates the given file with the data. You can repeat this command
whenever you want to have newer data. The command automatically picks up at
the same point where it left off after the previous update.

### Choosing the replication source

OSM files in PBF format are able to save the replication source and the
current status on their own. That is why pyosmium-up-to-date is able to
automatically do the right thing.
If you want to switch the replication source
or have a file that does not have replication information, you need to bootstrap
the update process and manually point `pyosmium-up-to-date` to the right
service:

    pyosmium-up-to-date --ignore-osmosis-headers --server <replication URL> <osmfile.osm.pbf>

`pyosmium-up-to-date` automatically finds the right sequence ID to use
by looking at the age of the data in your OSM file. It updates the file
and stores the new replication source in the file. The additional parameters
are then not necessary anymore for subsequent updates.

!!! Tip
    Always use the PBF format to store your data. Other format do not support
    to save the replication information. pyosmium-up-to-date is still able to
    update these kind of files if you manually point to the replication server
    but the process is always more costly because it needs to find the right
    starting point for updates first.

### Updating larger amounts of data

When used without any parameters, pyosmium downloads at a maximum about
1GB of changes. That corresponds to about 3 days of planet-wide changes.
You can increase the amount using the additional `--size` parameter:

    pyosmium-up-to-date --size=10000 planet.osm.pbf

This would download about 10GB or 30 days of change data. If your OSM data file is
older than that, downloading the full file anew is likely going to be faster.

`pyosmium-up-to-date` uses return codes to signal if it has downloaded all
available updates. A return code of 0 means that it has downloaded and
applied all available data. A return code of 1 indicates that it has applied
some updates but more are available.

A minimal script that updates a file until it is really up-to-date with the
replication source would look like this:

``` bash
status=1  # we want more data
while [ $status -eq 1 ]; do
    pyosmium-up-to-date planet.osm.pbf
    # save the return code
    status=$?
done
```

## Creating change files for updating databases

There are quite a few tools that can import OSM data into databases, for
example osm2pgsql, imposm or Nominatim. These tools often can use change files
to keep their database up-to-date. pyosmium can be used to create the appropriate
change files. This is slightly more involved than updating a file.

### Preparing the state file

Before downloading the updates, you need to find out with which sequence
number to start. The easiest way to remember your current status is to save
the number in a file. pyosmium can then read and update the file for you.

#### Method 1: Starting from the import file

If you still have the OSM file you used to set up your database, then
create a state file as follows:

    pyosmium-get-changes -O <osmfile.osm.pbf> -f sequence.state -v

Note that there is no output file yet. This creates a new file `sequence.state`
with the sequence ID where updates should start and prints the URL of the
replication service to use.

#### Method 2: Starting from a date

If you do not have the original OSM file anymore, then a good strategy is to
look for the date of the newest node in the database to find the snapshot date
of your database. Find the highest node ID, then look up the date for version 1
on the OSM website. For example the date for node 2367234 can be found at
[https://www.openstreetmap.org/api/0.6/node/23672341/1](https://www.openstreetmap.org/api/0.6/node/23672341/1)
Find and copy the `timestamp` field. Then create a state file using this date:

    pyosmium-get-changes -D 2007-01-01T14:16:21Z -f sequence.state -v

As before, this creates a new file `sequence.state` with the sequence ID where
updates should start and prints the URL of the replication service to use.

### Creating a change file

Now you can create change files using the state:

    pyosmium-get-changes --server <replication server> -f sequence.state -o newchange.osc.gz

This downloads the latest changes from the server, saves them in the file
`newchange.osc.gz` and updates your state file. `<replication server>` is the
URL that was printed when you set up the state file. The parameter can be
omitted when you use minutely change files from openstreetmap.org.
This simplifies multiple edits of the same element into the final change. If you want to
retain the full version history specify `--no-deduplicate`.

`pyosmium-get-changes` loads only about 100MB worth of updates at once (about
8 hours of planet updates). If you want more, then add a `--size` parameter.

### Continuously updating a database

`pyosmium-get-changes` emits special return codes that can be used to set
up a script that continuously fetches updates and applies them to a
database. The important error codes are:

 * 0 - changes successfully downloaded and new change file created
 * 3 - no new changes are available from the server

All other error codes indicate fatal errors.

A simple shell script can look like this:

``` bash
while true; do
  # pyosmium-get-changes would not overwrite an existing change file
  rm -f newchange.osc.gz
  # get the next batch of changes
  pyosmium-get-changes -f sequence.state -o newchange.osc.gz
  # save the return code
  status=$?

  if [ $status -eq 0 ]; then
    # apply newchange.osc.gz here
    ....
  elif [ $status -eq 3 ]; then
    # No new data, so sleep for a bit
    sleep 60
  else
    echo "Fatal error, stopping updates."
    exit $status
  fi
done
```
