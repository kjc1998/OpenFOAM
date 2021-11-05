# RANS Simulation of Centrifugal Pump

## SolidWorks CAD

<br/>

_Toy Model consisting of impeller, housing and cover with details stated in the table below_

> Impeller blade : guided loft to create curved surface
>
> Housing : Volute Casing via Surface Loft + thickness addition
>
> Cover : Direct fluid into blade structure

<table>
  <tr>
    <th><b>Impeller</b></th>
    <th><b>Housing</b></th>
    <th><b>Cover</b></th>
  </tr>
  <tr>
    <td><img src="https://user-images.githubusercontent.com/57020975/140510487-a4025029-c2e4-4bdb-a70c-ba1ffd5cd174.jpg" width=250vh height=250vh></td>
    <td><img src="https://user-images.githubusercontent.com/57020975/140510484-98da06b2-e4dd-4854-b7e9-a5b6c365d984.jpg" width=250vh height=250vh></td>
    <td><img src="https://user-images.githubusercontent.com/57020975/140510488-c53abe2a-a068-42ac-924c-4be2618e1d09.jpg" width=250vh height=250vh></td>
  </tr>
  <tr>
    <td>130 x 130 x 65 (mm3)</td>
    <td>90.01 x 303.93 x 306.85 (mm3)</td>
    <td>140 x 140 x 45 (mm3)</td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <th><b>Assembly View</b></th>
    <th><b>Inner View</b></th>
    <th><b>Section View</b></th>
  </tr>
  <tr>
    <td><img src="https://user-images.githubusercontent.com/57020975/140507038-e4ff2a48-ec83-4d61-b16f-e4aac3830948.jpg" width=250vh height=250vh></td>
    <td><img src="https://user-images.githubusercontent.com/57020975/140507032-a2d3e704-3578-429d-9ba2-7722dab5200b.jpg" width=250vh height=250vh></td>
    <td><img src="https://user-images.githubusercontent.com/57020975/140507034-4792155b-451b-478c-9ebf-2b1c1305f35f.jpg" width=250vh height=250vh></td>
  </tr>
 </table>

<br/>

## SnappyHexMesh Meshing

<br/>

_Key Settings_

> Setting Conical rotating zone in Mesh Dict

        rotatingAMI
        {
            level (2 4);
            faceType boundary;
            cellZone rotatingZone;	        // name of cell zone (for dynamic mesh)
            faceZone rotatingZone;	        // name of face zone (for dynamic mesh)
            cellZoneInside inside;
        }

> Refinement Level

        impeller{ level (3 3);}             // level (global refinement, curvature refinement)
        housing{ level (1 3);}
        cover{ level (0 0);}

> Edge and Surface Snapping Control

        snapControls
        {
            nSmoothPatch 3;
            tolerance 4.0;	                // maximum relative distance for shifting
            nSolveIter 300;                 // edge snapping iteration
            nRelaxIter 5;
            nFeatureSnapIter 10;
            implicitFeatureSnap true;       // dependent on resolve angle (better coupling for dynamic mesh)
            explicitFeatureSnap false;      // default (using extracted feature edges)
            multiRegionFeatureSnap false;   // detect features between surfaces
        }

**Screenshots**

<table>
  <tr>
    <td><img src="https://user-images.githubusercontent.com/57020975/140512307-76b8dd0b-fa90-4ac9-a884-b7d720874245.jpg" width=250vh height=250vh></td>
    <td><img src="https://user-images.githubusercontent.com/57020975/140512311-fdbbdfab-dc90-4a05-abea-a9a029646b47.jpg" width=250vh height=250vh></td>
    <td><img src="https://user-images.githubusercontent.com/57020975/140512309-1fa409de-b7c3-42b0-b64b-cd2e0f511626.jpg" width=250vh height=250vh></td>
  </tr>
</table>

<br/>

## RANS Simulation

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

> Schemes

        divSchemes                          // divergence schemes
        {
          default         	none;
          div(phi,U)      	Gauss CoBlended // Co dependent schemes
                            0.01
                            limitedLinearV 1
                            0.05
                            linearUpwind grad(U);
          div(phi,k)        Gauss linearUpwind grad(U);
          div(phi,K)        Gauss linearUpwind grad(U);
          div(phi,omega)    Gauss linearUpwind grad(U);
          div((nuEff*dev2(T(grad(U))))) Gauss linear;
        }
        ...

> Boundary Conditions

      U
        inlet
        {
            type            fixedValue;
            value           uniform (0.05 0 0);
        }
      ...

      k
        inlet
        {
            type            turbulentIntensityKineticEnergyInlet;
            intensity       0.05;           // 5% turbulence
            value           uniform 0.00325;
        }
      ...

      omega
        inlet
        {
            type            turbulentMixingLengthFrequencyInlet;
            mixingLength    0.015;
            value           uniform 2.6;
        }
      ...

<br/>

## Animation

<p align="center">
  <img src="https://user-images.githubusercontent.com/57020975/140513868-621f4a91-43aa-45dc-9924-0a0d6363d778.gif" height=400vh>
</p>
