# IFC2HCM: a tool for generating hospital configuration models from IFC BIM data models
IFC2HCM is a novel software tool designed to generate IndoorGML and Hospital Configuration Models (HCM) from IFC/BIM models. The primary motivation behind IFC2HCM is to develop a tool for generating HCM as the core foundation of the Hospital Design Support System (HDSS) that will evaluate hopsital layout design in terms of operational efficiency. The software addresses the need for detailed spatial network analysis and simulation modelling in complex hospital environments, offering a semi-automatic process to convert IFC data into IndoorGML, and subsequently into a comprehensive HCM. The HCM generated by this tool, consisted of geometric, topological, semantic, and operational information, supports applications such as space optimization, facility management, ensuring safety, and indoor navigation. 
## Software used: 
Python, Autodesk’s Revit and Dynamo, McNeel’s Rhino and Grasshopper
## Dependencies: 
Dependencies for Python: pandas, NumPy, COMPAS, Matplotlib, NetworkX, LXML. 
Dependencies for Grasshopper: Human, LunchBox, LunchBoxML.
## How to use: 
Step 1, Importing IFC. 
Open the open-source IFC file with Autodesk’s Revit. In Revit, open the Dynamo file `Home.dyn' which is located in `geometry\_software' directory of the repository. This step will produce two output files, one is for building's room boundaries, and another is for building's door locations.
    
Step 2, Generating IndoorGML. This step can be subdivided into five sub-steps. 
    
    Sub-step 1, \textbf{generating CellSpace for IndoorGML}. Put the room boundary output file from step 1 into `edit\_csv.ipynb' file located in `notes/csv processor' directory of the repository, and process the boundary file, then put the processed file into `add\_storey.ipynb' file to get the room lower boundary file. Next, put the room lower boundary file into `create\_upper\_boundary.ipynb' file to get room upper boundary file, then put both room lower boundary and upper boundary files into `create\_wall\_boundary.ipynb' to get wall boundary file. Lastly, put room lower boundary, upper boundary, and wall boundary files into `add\_all\_csBoundary\_together.ipynb' to create the CellSpace data.
    
    Sub-step 2, \textbf{generating CellSpaceBoundary for IndoorGML}. Put the door location output file from step 1 into `edit\_door\_csv.ipynb' located in `notes/csv processor' directory of the repository for processing the data and make it readable by Grasshopper, then put the processed data into the Grasshopper file named `create\_graph.gh' in `geometry\_software' directory of the repository, and create door boundaries. Next, put the door boundaries file into `edit\_door\_boundaries.ipynb' to get the final CellSpaceBoundary data.
    
    Sub-step 3, \textbf{generating nodes for IndoorGML}. In the room boundary file generated by step 1, select only corridor boundaries to obtain a corridor boundary file, and select only stair boundaries to obtain a stair boundary file. Then put the room boundary file, corridor boundary file, and stair boundary file into `edit\_csv.ipynb' to process them and make them readable by Grasshopper. Then put the processed files into the Grasshopper file `create\_graph.gh' to get nodes and edges data. Subsequently, put the nodes and edges data into `add\_edges\_to\_nodes.ipynb' located in `notes/csv processor' directory of the repository to add both groups of data together to obtain the final nodes file.

    Sub-step 4, \textbf{generating edges for IndoorGML}. In the last sub-step, from the Grasshopper file `create\_graph.gh', export all edges' start and end points to a csv file, and then put the csv file and final nodes file from last sub-step into `create\_transitions.ipynb' in `notes/csv processor' directory of the repository to generate final edge data.

    Sub-step 5, \textbf{generating IndoorGML}. Put all the four final outputs from previous sub-steps into `etree\_to\_gml.ipynb' in the `IndoorGML Generator' directory of the repository to obtain the IndoorGML file.
    
    \item \textbf{Step 3, Generating HCM.} Import the IndoorGML file from step 2 into the IndoorGML parser named `ig2ij.ipynb' in the `HCM generator' in the `notes/HCM generator' directory of the repository to get a JSON \cite{python_software_foundation_json_nodate} file, and then put the JSON file into `ij2cp\_and\_nx.ipynb' and `extract\_department\_and\_room.ipynb' files to obtain the final HCM.
