MemoryContentCache Class
========================

.. container:: experimental

    .. admonition:: Experimental

       The MemoryContentCache is experimental and the API is not finalized.

    A MemoryContentCache holds a set of Data packets and answers an Interest to
    return the correct Data packet. The cache is periodically cleaned up to
    remove each stale Data packet based on its FreshnessPeriod (if it has one).

    :[C++]:
        | ``#include <ndn-cpp/util/memory-content-cache.hpp>``
        | Namespace: ``ndn``

    :[Python]:
        Module: ``pyndn.util.memory_content_cache``

    :[Java]:
        Package: ``net.named_data.jndn.util``

MemoryContentCache Constructor
------------------------------

.. container:: experimental

    .. admonition:: Experimental

       The MemoryContentCache is experimental and the API is not finalized.

    Create a new MemoryContentCache to use the given Face.

    :[C++]:

        .. code-block:: c++

            MemoryContentCache(
                Face* face
                [, Milliseconds cleanupIntervalMilliseconds]
            );

    :[Python]:

        .. code-block:: python

            def __init__(self
                face                            # Face
                [, cleanupIntervalMilliseconds  # double]
            )

    :[JavaScript]:

        .. code-block:: javascript

            var MemoryContentCache = function MemoryContentCache(
                face                            // Face
                [, cleanupIntervalMilliseconds  // number]
            )

    :[Java]:

        .. code-block:: java

            public MemoryContentCache(
                Face face
                [, double cleanupIntervalMilliseconds]
            )

    :Parameters:

        - `face`
            The Face to use to call registerPrefix and which will call the OnInterest callback.

        - `cleanupIntervalMilliseconds`
            (optional) The interval in milliseconds
            between each check to clean up stale content in the cache. If omitted,
            use a default of 1000 milliseconds. If this is a large number, then
            effectively the stale content will not be removed from the cache.

MemoryContentCache.add Method
-----------------------------

.. container:: experimental

    .. admonition:: Experimental

       The MemoryContentCache is experimental and the API is not finalized.

    Add the Data packet to the cache so that it is available to use to 
    answer interests. If data.getFreshnessPeriod() is not negative, set the
    staleness time to now plus data.getFreshnessPeriod(), which is checked
    during cleanup to remove stale content.

    .. note::

        [except JavaScript] Your application must call :ref:`processEvents <processEvents>`.  
        Since processEvents modifies the cache, your application should make sure that it 
        calls processEvents in the same thread as add (which also modifies the cache).

    :[C++]:

        .. code-block:: c++

            void add(
                const Data& data
            );

    :[Python]:

        .. code-block:: python

            def add(self,
                data  # Data
            )

    :[JavaScript]:

        .. code-block:: javascript

            MemoryContentCache.prototype.add = function(
                data  // Data
            )

    :[Java]:

        .. code-block:: java

            public final void add(
                Data data
            )

    :Parameters:

        - `data`
            The Data packet object to put in the cache. This copies the 
            fields from the object.

MemoryContentCache.registerPrefix Method
----------------------------------------

.. container:: experimental

    .. admonition:: Experimental

       The MemoryContentCache is experimental and the API is not finalized.

    Call registerPrefix on the Face given to the constructor so that this
    MemoryContentCache will answer interests whose name has the prefix.

    .. note::

        [except JavaScript] Your application must call :ref:`processEvents <processEvents>`.  
        The cache is processed on the same thread that calls processEvents.

    :[C++]:

        .. code-block:: c++

            void registerPrefix(
                const Name& prefix,
                const OnRegisterFailed& onRegisterFailed
                [, const OnInterest& onDataNotFound]
                [, const ForwardingFlags& flags]
            );

    :[Python]:

        .. code-block:: python

            def registerPrefix(self,
                prefix,            # Name
                onRegisterFailed   # function object
                [, onDataNotFound  # function object]
                [, flags           # ForwardingFlags]
            )

    :[JavaScript]:

        .. code-block:: javascript

            MemoryContentCache.prototype.registerPrefix = function(
                prefix,            // Name
                onRegisterFailed   // function
                [, onDataNotFound  // function]
                [, flags           // ForwardingFlags]
            )

    :[Java]:

        .. code-block:: java

            public final void registerPrefix(
                Name prefix,
                OnRegisterFailed onRegisterFailed
                [, OnInterest onDataNotFound]
                [, ForwardingFlags flags]
            )

    :Parameters:

        - `prefix`
            The Name for the prefix to register. This copies the Name.

        - `onRegisterFailed`
            If failed to set Interest filter for any reason, this calls ``onRegisterFailed(prefix)`` where:

                - ``prefix`` is the prefix given to registerPrefix.

        - `onDataNotFound`
            (optional) This callback is called to forward the OnInterest message 
            when a data packet is not found in the cache. For details of the
            callback parameters, see the onInterest parameter of :ref:`registerPrefix <registerPrefix>`. 
            The onDataNotFound callback is called on the same thread that calls :ref:`processEvents <processEvents>`.
            If omitted, this does not use it.

        - `flags`
            (optional) The flags for finer control of how and which Interests should be forwarded towards the face.
            If omitted, use the default flags defined by the default :ref:`ForwardingFlags <ForwardingFlags>` constructor.

MemoryContentCache.unregisterAll Method
---------------------------------------

.. container:: experimental

    .. admonition:: Experimental

       The MemoryContentCache is experimental and the API is not finalized.

    Call Face.removeRegisteredPrefix for all the prefixes given to the
    registerPrefix method on this MemoryContentCache object so that it will not
    receive interests any more. You can call this if you want to "shut down"
    this MemoryContentCache while your application is still running.

    .. note::

        [except JavaScript] Your application should call this on the same thread
        that calls processEvents.

    :[C++]:

        .. code-block:: c++

            void unregisterAll();

    :[Python]:

        .. code-block:: python

            def unregisterAll(self)

    :[JavaScript]:

        .. code-block:: javascript

            MemoryContentCache.prototype.unregisterAll = function()

    :[Java]:

        .. code-block:: java

            public final unregisterAll()
