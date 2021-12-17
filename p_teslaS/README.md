# compressible RANS Simulation of Tesla car model S

## CAD

<br/>

> CAD and stl files obtained from <a href="https://grabcad.com/library/tesla-model-s-6">link</a>
>
> _Full credit to_ _<a href="https://grabcad.com/nathan.allen-17">Nathan Allen</a>_

<p align="center">
  <img src="https://user-images.githubusercontent.com/57020975/146573385-3314f9f7-96ca-4468-b70e-4f08d029009e.png" width=800vw height=500vw>
</p>

<br/>

## SnappyHexMesh Meshing

<br/>

_Key Settings_

> Setting Refinement Box in Geometry Section

        refinementBox
        {
            type    box;
            min     (-500 0 -10000);
            max     ( 3000  2000  5000);
        }

> Refinement Regions

        refinementSurfaces
        {
            car
            {
                // Surface-wise min and max refinement level
                level (2 3);
                patchInfo
                {
                  type wall;
                }
            }
        }

        refinementRegions
        {
            refinementBox
            {
                mode inside;
                levels ((1e15 2));
            }
        }

> Edge and Surface Snapping Control

        snapControls
        {
            //- Number of patch smoothing iterations before finding correspondence to surface
            nSmoothPatch 3;

            //- Relative distance for points to be attracted by surface feature point
            //  or edge. True distance is this factor times local maximum edge length.
            tolerance 4.0;

            //- Number of mesh displacement relaxation iterations.
            nSolveIter 30;

            //- Maximum number of snapping relaxation iterations. Should stop
            //  before upon reaching a correct mesh.
            nRelaxIter 5;
            nFeatureSnapIter 5;

            implicitFeatureSnap false; // dependent on resolve angle
            explicitFeatureSnap true;  // default (using extracted feature edges)
            multiRegionFeatureSnap true; // detect features between surfaces
        }

**Screenshots**

<table>
  <tr>
    <td><img src="https://user-images.githubusercontent.com/57020975/146574358-fc91b7df-bc7a-4855-ab30-f8033480f014.png" width=400vw height=250vw></td>
    <td><img src="https://user-images.githubusercontent.com/57020975/146574360-214fa0e4-ed9f-4adc-9a61-0c5b07f0feaa.png" width=400vw height=250vw></td>
  </tr>
</table>

<br/>

## Compressible RANS Simulation

<br/>

_Key Settings_

> Turbulence Model

        simulationType RAS;

        RAS
        {
            RASModel        kOmegaSST;
            turbulence      on;
            printCoeffs     on;
        }

> Thermo Physical Properties

        thermoType
        {
            type            hePsiThermo;
            mixture         pureMixture;
            transport       const;
            thermo          hConst;
            equationOfState perfectGas;
            specie          specie;
            energy          sensibleInternalEnergy;
        }

        mixture // air at room temperature (293 K)
        {
            specie
            {
                molWeight   28.9;
            }
            thermodynamics
            {
                Cp          1005;
                Hf          0;
            }
            transport
            {
                mu          1.82e-05;
                Pr          0.71;
            }
        }

> Schemes

        divSchemes
        {
            default         none;

            div(phi,U)      Gauss linearUpwind limited;

            // energy
            div(phi,e)      Gauss linearUpwind limited;
            div(phi,K)      Gauss linearUpwind limited;
            div(phi,Ekp)    Gauss linearUpwind limited;

            // turbulence
            div(phi,k)      Gauss linearUpwind limited;
            div(phi,omega)  Gauss linearUpwind limited;

            div(phiv,p)     Gauss upwind;
            div((phi|interpolate(rho)),p) 	Gauss upwind;

            div(((rho*nuEff)*dev2(T(grad(U)))))	Gauss linear;
        }
        ...

> Temperature Options

        limitT
        {
            type       limitTemperature;
            min        101;
            max        1000;
            selectionMode all;
        }

> Boundary Conditions

      U
        inlet
        {
            type            fixedValue;
            value		        uniform (0 0 -55); // 198 km/h
        }
      ...

      k
        inlet
        {
            type            inletOutlet;
            inletValue      uniform 10; // setting based on standard turbulence parameter calculation
            value           uniform 10;
        }
      ...

      omega
        inlet
        {
            type            inletOutlet;
            inletValue      uniform 10; // setting based on standard turbulence parameter calculation
            value           uniform 10;
        }
      ...

<br/>

## Animation

<div style="text-align: center;">
  <table style="margin: 0 auto">
    <tr>
      <td><img src="https://user-images.githubusercontent.com/57020975/146577829-1e17a52a-086b-476e-942e-6a4c7e219d48.gif" height=300vw></td>
      <td><img src="https://user-images.githubusercontent.com/57020975/146577565-a5ef4ca4-1c1c-487e-881c-e1274007f627.gif" height=300vw></td>
    </tr>
    <tr>
      <td>Streamline (Velocity and Pressure)</td>
      <td>Slice Plot (Velocity and Turbulence Intensity)</td>
    </tr>
  </table>
</div>
