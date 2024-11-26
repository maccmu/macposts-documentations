# MNM_nb Documentation

This document provides an overview and documentation for the classes defined in the `MNM_nb.py` file. Each class is described with its attributes and methods.

## Classes

### MNM_dlink

A class to represent a network link in a traffic simulation model.

#### Attributes
- `ID` (int): The unique identifier for the link.
- `length` (float): The length of the link in miles.
- `typ` (str): The type of the link.
- `ffs` (float): The free-flow speed of the link in miles per hour.
- `cap` (float): The capacity of the link in vehicles per hour.
- `rhoj` (float): The jam density of the link in vehicles per mile.
- `lanes` (int): The number of lanes on the link.

#### Methods
- `get_fft()`: Calculates and returns the free-flow travel time of the link in seconds.
- `is_ok(unit_time=5)`: Validates the attributes of the link.
- `__str__()`: Returns a string representation of the link.
- `__repr__()`: Returns a string representation of the link.
- `generate_text()`: Generates a space-separated string of the link's attributes.

### MNM_dnode

Class representing a directed node (MNM_dnode).

#### Attributes
- `ID` (int or None): The identifier of the node.
- `typ` (str or None): The type of the node.

#### Methods
- `__init__(ID, typ)`: Initializes a new instance of MNM_dnode with the given ID and type.
- `is_ok()`: Checks if the node type is valid by asserting it is in DNODE_ENUM.
- `__str__()`: Returns a string representation of the node.
- `__repr__()`: Returns a string representation of the node (same as `__str__()`).
- `generate_text()`: Generates a text representation of the node attributes.

### MNM_demand

A class to represent and manage traffic demand data.

#### Attributes
- `demand_dict` (dict): A dictionary to store demand data with origin (O) as keys and another dictionary as values where destination (D) is the key and demand is the value.
- `demand_list` (list): A list to store tuples of (O, D) representing origin and destination pairs.

#### Methods
- `add_demand(O, D, demand, overwriting=False)`: Adds demand data for a given origin (O) and destination (D). Raises an error if the demand already exists and overwriting is set to False.
- `build_from_file(file_name)`: Reads demand data from a file and populates `demand_dict` and `demand_list` attributes.
- `__str__()`: Returns a string representation of the MNM_demand object showing the number of origins and OD pairs.
- `__repr__()`: Returns the string representation of the MNM_demand object.
- `generate_text()`: Generates a text representation of the demand data in a specific format.

### MNM_od

A class to represent the origin-destination (OD) mapping in a network.

#### Attributes
- `O_dict` (bidict): A bidirectional dictionary to store origin nodes and their corresponding IDs.
- `D_dict` (bidict): A bidirectional dictionary to store destination nodes and their corresponding IDs.

#### Methods
- `add_origin(O, Onode_ID, overwriting=False)`: Adds an origin node to the `O_dict`. Raises an error if the origin node already exists and overwriting is False.
- `add_destination(D, Dnode_ID, overwriting=False)`: Adds a destination node to the `D_dict`. Raises an error if the destination node already exists and overwriting is False.
- `build_from_file(file_name)`: Builds the `O_dict` and `D_dict` from a given file.
- `generate_text()`: Generates a text representation of the `O_dict` and `D_dict`.
- `__str__()`: Returns a string representation of the MNM_od object.
- `__repr__()`: Returns a string representation of the MNM_od object.

### MNM_graph

A class to represent a directed graph using NetworkX.

#### Attributes
- `G` (nx.DiGraph): A directed graph object from NetworkX.
- `edgeID_dict` (OrderedDict): A dictionary to store edge IDs and their corresponding start and end nodes.

#### Methods
- `add_edge(s, e, ID, create_node=False, overwriting=False)`: Adds an edge to the graph.
- `add_node(node, overwriting=True)`: Adds a node to the graph.
- `build_from_file(file_name)`: Builds the graph from a file.
- `__str__()`: Returns a string representation of the graph.
- `__repr__()`: Returns a string representation of the graph.
- `generate_text()`: Generates a text representation of the graph's edges.

### MNM_path

MNM_path class represents a path in a network with a unique path ID, origin node, destination node, and a list of nodes that form the path. It also includes route portions for route choice modeling.

#### Attributes
- `path_ID` (int): Unique identifier for the path.
- `origin_node` (int): The starting node of the path.
- `destination_node` (int): The ending node of the path.
- `node_list` (list): List of nodes that form the path.
- `route_portions` (numpy.ndarray): Array representing route choice portions.

#### Methods
- `__init__(node_list, path_ID)`: Initializes the MNM_path with a list of nodes and a path ID.
- `__eq__(other)`: Checks if two MNM_path objects are equal based on their attributes.
- `create_route_choice_portions(num_intervals)`: Creates an array of ones with the specified number of intervals for route portions.
- `attach_route_choice_portions(portions)`: Attaches an array of route choice portions to the path.
- `__ne__(other)`: Checks if two MNM_path objects are not equal.
- `__str__()`: Returns a string representation of the MNM_path object.
- `__repr__()`: Returns a string representation of the MNM_path object.
- `generate_node_list_text()`: Generates a text representation of the node list.
- `generate_portion_text()`: Generates a text representation of the route portions.

### MNM_pathset

A class to represent a set of paths between an origin and a destination node.

#### Attributes
- `origin_node` (None or node type): The origin node of the path set.
- `destination_node` (None or node type): The destination node of the path set.
- `path_list` (list): A list to store paths in the path set.

#### Methods
- `add_path(path, overwriting=False)`: Adds a path to the path set. Raises an error if the path already exists and overwriting is False.
- `normalize_route_portions(sum_to_OD=False)`: Normalizes the route portions of the paths in the path set. If `sum_to_OD` is True, returns the sum of the route portions.
- `__str__()`: Returns a string representation of the path set.
- `__repr__()`: Returns a string representation of the path set.

### MNM_pathtable

A class to represent a path table for a transportation network.

#### Attributes
- `path_dict` (dict): A dictionary to store path sets, indexed by origin and destination nodes.
- `ID2path` (OrderedDict): An ordered dictionary to store paths indexed by their IDs.

#### Methods
- `add_pathset(pathset, overwriting=False)`: Adds a path set to the path table.
- `build_from_file(file_name, w_ID=False)`: Builds the path table from a file.
- `load_route_choice_from_file(file_name, w_ID=False)`: Loads route choice portions from a file and attaches them to paths.
- `__str__()`: Returns a string representation of the path table.
- `__repr__()`: Returns a string representation of the path table.
- `generate_table_text()`: Generates a text representation of the path table.
- `generate_portion_text()`: Generates a text representation of the route choice portions.

### MNM_config

A class to represent the configuration for MNM (Macroscopic Network Model).

#### Attributes
- `config_dict` (OrderedDict): A dictionary to store configuration parameters.
- `type_dict` (dict): A dictionary to map configuration parameter names to their types.

#### Methods
- `__init__()`: Initializes the MNM_config object with default values.
- `build_from_file(file_name)`: Builds the configuration dictionary from a given file.
- `__str__()`: Returns a string representation of the configuration.
- `__repr__()`: Returns a string representation of the configuration (same as `__str__()`).

### MNM_network_builder

A class used to build and manage a transportation network.

#### Attributes
- `config` (MNM_config): Configuration settings for the network.
- `network_name` (str or None): The name of the network.
- `link_list` (list): A list of links in the network.
- `node_list` (list): A list of nodes in the network.
- `graph` (MNM_graph): The graph representation of the network.
- `od` (MNM_od): The origin-destination pairs in the network.
- `demand` (MNM_demand): The demand data for the network.
- `path_table` (MNM_pathtable): The table of paths in the network.
- `route_choice_flag` (bool): A flag indicating if route choice portions are loaded.

#### Methods
- `get_link(ID)`: Returns the link with the specified ID.
- `load_from_folder(path, config_file_name='config.conf', link_file_name='MNM_input_link', node_file_name='MNM_input_node', graph_file_name='Snap_graph', od_file_name='MNM_input_od', pathtable_file_name='path_table', path_p_file_name='path_table_buffer', demand_file_name='MNM_input_demand')`: Loads network data from the specified folder.
- `dump_to_folder(path, config_file_name='config.conf', link_file_name='MNM_input_link', node_file_name='MNM_input_node', graph_file_name='Snap_graph', od_file_name='MNM_input_od', pathtable_file_name='path_table', path_p_file_name='path_table_buffer', demand_file_name='MNM_input_demand')`: Dumps network data to the specified folder.
- `read_link_input(file_name)`: Reads link data from the specified file.
- `read_node_input(file_name)`: Reads node data from the specified file.
- `generate_link_text()`: Generates text representation of the link data.
- `generate_node_text()`: Generates text representation of the node data.
- `set_network_name(name)`: Sets the name of the network.
- `update_demand_path(f)`: Updates the demand path with the specified data.
- `get_path_flow()`: Returns the path flow data.
- `get_route_portion_matrix()`: Returns the route portion matrix.