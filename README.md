# OpenFOAM

OpenFOAM Projects

- Corner Flow (Turbulent -> Steady State)
- Block Mesh Test Folder
- Flat Plate
- Cylinder (Laminar)
- Cylinder (Turbulent, additional option for parallel run)
  - RANS pimpleFoam for kEpsilon, kOmegaSST, spalartAllmaras
  - LES pimpleFoam for kOmegaSSTIDDES, Smagorinsky
- Karman Vortex (snappyHexMesh from stl, realisable k-epsilon) [requires dynamic code]
- rotatingFanInRoom (referece model for pumpModel)
- pumpModel (solidworks CAD design -> snappyHexMesh -> AMI mesh settings -> kOMegaSST RANS modelling)
