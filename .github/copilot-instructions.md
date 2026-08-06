## Copilot instructions for NetApp SMI-S Provider documentation

### Repository overview
Product: NetApp SMI-S Provider

NetApp SMI-S Provider is a command-based interface for detecting, managing, and monitoring NetApp storage systems that run ONTAP. It exposes storage management through Web-Based Enterprise Management (WBEM) standards and command-line tooling around a CIM server.

### Repository structure
- `media/` – Images used by architecture and workflow topics.
- `redirect/` – Redirect stub pages that map older permalinks to current documentation URLs.

### Product-specific context
**Architecture and components:**
- *CIMOM* is the request-handling foundation; it validates and authenticates client requests, then invokes the correct provider implementation.
- *Provider objects* are shared library implementations loaded by CIMOM to process commands and query storage systems through device-specific APIs.
- *Repository* is a flat-file CIM-level data store used by CIMOM for persistent management data.
- WBEM clients discover services through Service Location Protocol *(SLP)* and communicate with the CIM server using *CIM-XML* over HTTPS (HTTP is also supported).

**Key concepts:**
- The *CIMOM repository* is where managed storage systems are registered for SMI-S operations and cache-backed queries.
- Core operational commands are in the *`smis`* namespace for adding, listing, refreshing, and removing storage systems and for reporting managed objects (for example disks, LUNs, pools, volumes, and exports).
- CIM server configuration is handled with *`cimconfig`*, and CIM user and trust-store administration is handled with *`cimuser`*.
- Indications are controlled by the *`PEGASUS_DISABLE_INDICATIONS`* environment variable and include Alert, FileSystem Quota, and Lifecycle indication classes.

**Naming conventions and terminology:**
- *SVM* means Storage Virtual Machine, and storage systems are added using an SVM management IP plus SVM credentials.
- NetApp SMI-S Provider does not support cluster IP addresses, node management IP addresses, node admin, or node SVMs for storage-system add operations.
- Command naming follows fixed patterns such as *`smis <subcommand>`*, *`cimconfig -s <name>=<value> -p`*, and service-control actions like *`smis cimserver restart`*.
- *SCVMM* topics refer to System Center Virtual Machine Manager discovery and rescan behavior through SMI-S Provider.

### Typical user workflows
**Initial deployment and validation:** Download software package → Install SMI-S Provider on a Windows host → Verify CIM server status → Add CIM server user → Add at least one storage system to the CIMOM repository → Verify managed object output with `smis` inventory commands

**Secure CIM server connectivity:** Access SMI-S Provider host session as administrator → Disable HTTP and enable HTTPS with `cimconfig` → Adjust CIM server ports if needed → Restart CIM server → Re-validate client connectivity

**Storage-system lifecycle management:** Add storage systems with `smis add` or `smis addsecure` → Confirm registration with `smis list` → Inspect exports/LUNs/pools/volumes for expected inventory → Refresh cache state as needed → Remove unused storage systems from the repository
