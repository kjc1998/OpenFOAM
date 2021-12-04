OpenFOAM Projects:

- flatPlate (Flat plate simulations)
- Karman Vortex (SnappyHexMesh from stl, realisable k-epsilon) [requires dynamic code]
- pipeFlowAnalysis
  - pimpleFoamPipe (Turbulent cylinder, additional option for parallel run)
    - RANS pimpleFoam for kEpsilon, kOmegaSST, spalartAllmaras
    - LES pimpleFoam for kOmegaSSTIDDES, Smagorinsky
  - pisoFoamPipe (Turbulent pipe flow with pisoFoam solver)
  - snappyPipe (Laminar pipeflow with snappyHexMesh)
- pumpModel ([Details](https://github.com/kjc1998/OpenFOAM/tree/master/pumpModel))
- testFiles (Meshing testing folder)
- windTurbine (wind turbine simulation coupled with FSI WIP)

Reference Folders:

- cornerFlow (Turbulent solver under steady state)
- cylinderPotential (PotentialFoam setups)
- plateHole (Tutorial folder)
- rotatingFanInRoom (Referece model for pumpModel)
- FSI (Fluid Structure Interaction for Wind Turbine project)
