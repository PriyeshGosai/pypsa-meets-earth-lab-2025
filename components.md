# PyPSA Components Reference

```{admonition} Content Source & Last Updated
:class: note
This page contains content from the [PyPSA Documentation](https://pypsa.readthedocs.io/en/latest/user-guide/components.html).
**Official Documentation:** [https://pypsa.readthedocs.io](https://pypsa.readthedocs.io)
**GitHub Source:** [PyPSA/PyPSA/docs/user-guide/components](https://github.com/PyPSA/PyPSA/tree/master/docs/user-guide/components)
**Last updated:** 2025-11-20
**License:** CC-BY-4.0 (PyPSA Contributors)
```

---

# Components Object

[`pypsa.Components`](https://pypsa.readthedocs.io/) are the store for all component specific data. While a [`pypsa.Network`](https://pypsa.readthedocs.io/) bundles together functionality across components, the `Components` class is the interface for all data and processing for a specific component type.

```python
import pypsa
n = pypsa.examples.ac_dc_meshed()
n.components.generators
```

```{tip}
A short name, such as `c`, is recommended since it is used frequently to access the stored data, properties and methods.
```

## Components Store

Components can be accessed in any `Network` via the `Components` store `n.components`. The `Components` store is a dict-like object that contains all components of the network.

```{note}
There is also an alias `n.c` for `n.components`.
```

**Access a single component:**
```python
c = n.components.generators
# or
c = n.components["Generator"]  # also subscriptable
```

**Access a list of components:**
```python
comps = n.components["Generator", "Bus"]
```

**Loop through all components:**
```python
for comp in n.components:
    break
```

```{important}
Even if assigned to a variable, the components are not copied. They are still attached to the network. If you change any data of the components object, the changes will be reflected in the network as well. There is no need to re-assign the components object to the network.
```

## Stored Data

All components data is stored in the two stores `c.static` and `c.dynamic`:

### Static Data

`c.static` contains all static data of the components, i.e. data that does not change over time. It is a simple `pandas.DataFrame` with the component names as index and all attributes as columns. E.g. for generators this includes `bus`, `carrier`, `p_nom`, etc.

```python
c = n.components.generators  # Set c to generators for examples
c.static
#                         bus control  ...
# name                                 ...
# Manchester Wind  Manchester      PQ  ...
# Manchester Gas   Manchester      PQ  ...
```

### Dynamic Data

`c.dynamic` contains all time-varying data of the component. It is a dict-like object that contains a `pandas.DataFrame` for each time-varying attribute. E.g. `p` or `p_max_pu`.

```python
c.dynamic.p_max_pu
# name                 Manchester Wind  Frankfurt Wind  Norway Wind
# snapshot
# 2015-01-01 00:00:00         0.930020        0.559078     0.974583
# 2015-01-01 01:00:00         0.485748        0.752910     0.481290
```

Stochastic Networks and multi-period networks use the same structure, but add additional dimensions to the dataframes.

```{note}
`c.static` and `c.dynamic` are exactly the same than `n.generators` and `n.generators_t`, which is still the default way to access the data and used in most of this documentation and any examples previous to version `v1.0`.
```

## Features

[`pypsa.Components`](https://pypsa.readthedocs.io/) have been introduced as experimental in v0.33.0 and are now released as stable in v1.0.0. They do not change anything in the underlying data structure of static and dynamic data stored in `pandas` DataFrames.

However, they add a lot of additional functionality that would otherwise have to be reimplemented on the underlying `pandas` DataFrames repeatedly. Using them reduces the need for boilerplate code, while still allowing users to continue using only the underlying DataFrames directly.

For a list of features, checkout the [API documentation](https://pypsa.readthedocs.io/).

```{note}
More features will be added in future releases. If you have any suggestions or requests, please open an issue on [GitHub](https://github.com/PyPSA/PyPSA/issues).
```

## New Components Class API

Prior to version v0.33.0 components data was only available in the two data stores `n.generators` and `n.generators_t`, which were directly attached to the network and not linked. With v1.0.0 they are still available in the same way, but now also via the newly introduced class. A new **optional** breaking API is introduced to make the usage of components more intuitive.

The current components API works as follows:

- `n.generators` → reference to `n.components.generators.static`
- `n.generator_t` → reference to `n.components.generators.dynamic`

To get access to the full functionality of `pypsa.Components` the long namespace must be used or assigned to a variable. For example, to rename a component across all dataframes or add components with attribute type hints, the following code must be run:

```python
n.components["Generator"].rename_component_names(bus1="bus_renamed")
c = n.components["Generator"]
c.add(name="New Gen", bus="bus_renamed", p_nom=100, carrier="wind")
```

### Opt-in to new API

Therefore, PyPSA v1.0 now allows you to opt in to an alternative, new Components API, which changes the reference to:

| Namespace | Current API | Opt-in API |
|-----------|--------------|------------|
| `n.generators` | `n.components.generators.static` | `n.components.generators` |
| `n.generators_t` | `n.components.generators.dynamic` | Deprecated |

With the new API, working with components should become more intuitive. The relationship between static and dynamic data is clearer and the numerous features of the `Components` class are faster to access with full support for auto-completion in IDEs when adding components with `n.generators.add(...)`.

One downside is that accessing the main static dataframe with `n.generators.static` instead of `n.generators` changes and is a bit longer. However, using a variable (e.g. `c`) as reference is still possible. Additionally, using the new API requires changes in existing code bases.

### Migrating to new API

To make the migration as easy as possible, and to also allow step-by-step migration, the options module is used.

As a quick example, let's take a snippet of the Three-Node Capacity Expansion Example.

```python
# Interconnector Capacities
n.links.query("carrier == 'HVDC'").p_nom_opt.round(2)

# Interconnector Flows
n.links_t.p0.loc[:, n.links.carrier == "HVDC"].rolling("7d").mean()
```

To switch to the new API, we just need to set the package option `api.new_components_api` to `True`.

```python
pypsa.options.api.new_components_api = True

# Interconnector Capacities
n.links.static.query("carrier == 'HVDC'").p_nom_opt.round(2)

# Interconnector Flows
n.links.dynamic.p0.loc[:, n.links.static.carrier == "HVDC"].rolling("7d").mean()

# Now you can also use the full functionality of the Components class, for example:
n.links.additional_ports
```

#### Step-by-step migration

To migrate a script step by step, we can just set the option back again.

```python
pypsa.options.api.new_components_api = True

# Interconnector Capacities
n.links.static.query("carrier == 'HVDC'").p_nom_opt.round(2)

# Switch back to old API
pypsa.options.api.new_components_api = False

# Interconnector Flows
n.links_t.p0.loc[:, n.links.carrier == "HVDC"].rolling("7d").mean()
```

Another way is to use the `option_context` context manager to temporarily switch the API.

```python
with pypsa.options_context(api.new_components_api=True):
    # Interconnector Capacities
    n.links.static.query("carrier == 'HVDC'").p_nom_opt.round(2)

# Interconnector Flows
n.links_t.p0.loc[:, n.links.carrier == "HVDC"].rolling("7d").mean()
```

PyPSA v1.0 will support full functionality for both APIs. The example above just shows how to immediately translate between the two APIs. It often also makes sense to assign the components object to a variable, which is then used in the rest of the script.

```{note}
With v2.0 of PyPSA, there are ongoing discussions to enable the new API by default with an opt-out option to still support old implementations.

We are happy to receive feedback on this planned change of the API. Please open an issue on [GitHub](https://github.com/PyPSA/PyPSA/issues) or join the shared [Discord server](https://discord.gg/AnuJBk23FU).
```

---

## Component Types

PyPSA supports various component types for modeling energy systems. Each component type has specific attributes and functionality:

### Core Components

- **Buses** - Network nodes where components are connected
- **Carriers** - Energy carriers (e.g., electricity, gas, heat)
- **Generators** - Power generation units
- **Loads** - Energy consumption points
- **Storage Units** - Energy storage systems
- **Stores** - State-of-charge stores

### Network Components

- **Lines** - AC transmission lines
- **Links** - General connections (DC lines, sector coupling)
- **Transformers** - Voltage transformation

### Additional Components

- **Line Types** - Standard line parameters
- **Shunt Impedances** - Reactive power compensation
- **Global Constraints** - System-wide constraints
- **Shapes** - Geographical boundaries

```{seealso}
For detailed information about each component type, visit the [PyPSA Documentation](https://pypsa.readthedocs.io/).
```

---

```{admonition} Links
:class: tip
- [PyPSA GitHub Repository](https://github.com/PyPSA/PyPSA)
- [PyPSA Documentation](https://pypsa.readthedocs.io/)
- [PyPSA-Earth Project](https://github.com/pypsa-meets-earth)
```
