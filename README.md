# QCU
Quantum Computing Unity allows to make a call to a real quantum computer from within unity

This project is meant to be used together with Unity3d (https://unity3d.com)

## Setup

WINDOWS and MACOS SETUP

Download a local copy of this project to a folder which is in a not too long filepath. (Else it could cause problems with builds).

In order to run QCU you need to first setup a python environment with qiskit installed.
One way to do this is, is by using anaconda with the following commands

conda create -n qiskitEnvironment python=3.8

conda activate qiskitEnvironment

pip install qiskit


For more info: https://qiskit.org/documentation/install.html

After you have created this environment, you can find it in your anaconda installation: https://docs.anaconda.com/anaconda/user-guide/faq/

In that folder there will be a folder "env" and inside there will be a folder "qiskitEnvironment" (if you named it like this).
Copy all the content of the folder "qiskitEnvironment" into your local copy of QCU into the folder Assets/StreamingAssets/.q 
You need to do the copying in your file browser, not in Unity, since folders starting with a dot are hidden in Unity (and not processed).
You may need to change the setting to show hidden folders/files to be able to see the .q folder.
(The folder will be empty except a PASTE_YOUR_FILES_HERE.txt)

## Using QCU

### Examples

#### Running a Python Script
Under Assets/Examples there is the Scene PythonRunningExample. In the Scene there is an Object QiskitExample with the Script PythonScriptTest.cs on it.
That script contains example code on how to run a python file and reads its output.

There are two examples one for how to do this threaded and one how to do it directly. 
The direct one is just there as an example we do not recomend to use it, since it will block the unity main thread, so you will notice a freeze while it is running.

When you press start the in unity the python scrupt will run automatically. If you uncheck the "Run Threaded" it wil run directly instead of in a seperate thread.

#### Running Qiskit

Under Assets/Examples there is the Scene QiskitExample.unity. In the Scene there is an Object QiskitExample with the Script RealQiskitBlurExample.cs on it.
That script contains example code on how to use QCU to run code on a real quantum computer.

To try it you can change the settings on the QiskitExample object. NOTE if you want to use a real quantum device instead of the qiskit simulator, you need to Provide your AuthentificationToken. You can copy paste it into the "Token" field of the "QiskitExample" Gameobject.

When you press play the script will automatically run. This example transforms an image (Texture) into a quantum circuit and applies a quantum blur to it.
Then it calculates the probabilities (using qiskit simulator or a real backend) and from the probabilities it generates the blurred image.

#### Reading the results from qiskit
Under Assets/Examples there is the Scene ReverseExample. In the Scene there is an Object Reverse with the Script ReverseImageConstructor.cs on it.
That script contains two example how to read data generated by qiskit either by a real quantum computer or a quasm simulator.

You can choose if you want to only run a single file (Set UseSingleFile to through (can be done in the editor)), then it uses the "Single File".
Else it will use all the files under "Multiple Files".

To use your own files, just drag and drop them first to unity as some sort of textfiles. And in Unity you can drag and drop them into "Single File" or "Multiple Files".

You can play around with your own Normalization, or just recalculate a normalization. This is, in our example, used mainly because we construct a black and white image from the results. If you want no renormalization you can set the value to 1.


### General Usage

As you can see in the example there are these steps which needs to be done:

1. Create a new SimulateJob
2. Create a new QuantumCircuit with the needed number of qubits
  - If possible use no initialisation for the amplitudes, since this is slow on real devices
  - Add gates to the circuit (X, H, RX, CX, CRX gates are supported at the moment)
3. Create a new QiskitSimulator
  - Set the numberOfShots (number of times the circuit is run)
  - Set useRealMachine to true if you want to use a real device (or false if not)
  - Set the authToken to your authentification token for the IBM backend (if useRealMachine)
  - Specify the backend used (if you want a specific one)
4. Prepare the SimulateJob by setting the circuit and the simulator.
5. Start the SimulateJob
6. Wait for the SimulateJob to finish (in an Update)
7. When the Job is finished get the calculated probabilities from your job to further useage.


## Making Builds

You can only make builds for the platform you are currently using, since the Python Environment is platform specific.

Further make sure the file path to the folder where you have the local copy of this repository is not too long, else build errors could come up.
(This happened on our testing on windows machines, and this is the reason why the project name and the folder for the python environment is now so short).

Except from the above mentioned details building works the same as it always does with unity: https://docs.unity3d.com/Manual/PublishingBuilds.html
