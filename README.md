OpenFOAM Projects:

- flatPlate (Flat plate simulations)
- Karman Vortex (SnappyHexMesh from stl, realisable k-epsilon) [requires dynamic code]
- pimpleFoamPipe (Turbulent cylinder, additional option for parallel run)
  - RANS pimpleFoam for kEpsilon, kOmegaSST, spalartAllmaras
  - LES pimpleFoam for kOmegaSSTIDDES, Smagorinsky
- pisoFoamPipe (Turbulent pipe flow with pisoFoam solver)
- [pumpModel](https://github.com/kjc1998/OpenFOAM/tree/master/pumpModel)
  (Solidworks CAD design -> snappyHexMesh -> AMI mesh settings (lowWeightCorrection factor included) -> kOMegaSST RANS modelling)
- snappyPipe (Laminar pipeflow with snappyHexMesh)
- testFiles (Meshing testing folder)


Reference Folders:

- cornerFlow (Turbulent solver under steady state)
- cylinderPotential (PotentialFoam setups)
- plateHole (Tutorial folder)
- rotatingFanInRoom (Referece model for pumpModel)
