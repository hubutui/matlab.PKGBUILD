# Maintainer: Grey Christoforo <first name at last name dot net>
# Modified by: Darcy Hu <hot123tea123@gmail.com>

## This PKGBUILD creates an Arch Linux package for the proprietary MATLAB application. A license from The MathWorks is needed in order to both build the package and to run MATLAB once the package is installed. In order to build the package the user must supply a plain text file installation key and the software. For network installations, in addition to the file installation key, a license file needs to be used for the installation. The tar archive file can be generated from an ISO downloaded from The MathWorks, generated from the official DVD, or created by using the interactive installer to download the toolboxes (installation can be made to a temporary directory and canceled once the toolboxes are downloaded). The contents of the tar archive must include: ./archives/ ./bin/ ./etc/ ./help/ ./java/ /sys ./activate.ini ./install ./installer_input.txt

## The default installation behavior is to install all licensed products whether or not they are available in the tar file. To install only a subset of licensed products either provide a $_products array or set $_partialinstall and remove unwanted entries from the provided $_products array. To perform a network install set $_networkinstall.

pkgname=matlab
pkgver=9.4.0.813654
pkgrel=1
_instdir="/opt/${pkgname}"
pkgdesc='A high-level language for numerical computation and visualization'
arch=('x86_64')
url='http://www.mathworks.com'
license=(custom)
depends=('glu')
optdepends=("gcc6: For MEX support"
		"gcc6-fortan: For MEX support")
source=("file://matlab.tar"
        "file://matlab.fik")
md5sums=('SKIP'
         'SKIP')
PKGEXT='.pkg.tar'

#_networkinstall=true

## For network installations, apparently, a license file needs to be used for the installation.
if [ ! -z ${_networkinstall+isSet} ]; then
  source+=("file://license.dat")
  md5sums+=('SKIP')
fi

prepare() {
  msg2 'Extracting file installation key'
  _fik=$(grep -o [0-9-]* ${pkgname}.fik)

  msg2 'Modifying the installer settings'
  sed -i "s,^# destinationFolder=,destinationFolder=${pkgdir}/${_instdir}," "${srcdir}/${pkgname}/installer_input.txt"
  sed -i "s,^# agreeToLicense=,agreeToLicense=yes," "${srcdir}/${pkgname}/installer_input.txt"
  sed -i "s,^# mode=,mode=silent," "${srcdir}/${pkgname}/installer_input.txt"
  sed -i "s,^# fileInstallationKey=,fileInstallationKey=${_fik}," "${srcdir}/${pkgname}/installer_input.txt"
  if [ ! -z ${_networkinstall+isSet} ]; then
    sed -i "s,^# licensePath=,licensePath=${srcdir}/license.dat," "${srcdir}/${pkgname}/installer_input.txt"
  fi
  if [ ! -z ${_products+isSet} ]; then
    msg2 'Building a package with a subset of the licensed products.'
    for _product in "${_products[@]}"; do
      sed -i "/^#product.${_product}$/ s/^#//" "${srcdir}/${pkgname}/installer_input.txt"
    done
  fi
}

package() {
  msg2 'Starting MATLAB installer'
  "${srcdir}/${pkgname}/install" -inputFile "${srcdir}/${pkgname}/installer_input.txt"

  msg2 'Installing license'
  install -D -m644 "${pkgdir}/${_instdir}/license_agreement.txt" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  msg2 'Creating links for executables'
  install -d -m755 "${pkgdir}/usr/bin/"
  for _executable in deploytool matlab mbuild mcc mex; do
    ln -s "/opt/${pkgname}/bin/${_executable}" "${pkgdir}/usr/bin/${_executable}"
  done

  msg2 'Installing desktop files'
#install -D -m644 "${pkgname}.desktop" "${pkgdir}/usr/share/applications/${pkgname}.desktop"
  install -D -m644 "${pkgdir}/opt/${pkgname}/help/matlab/matlab_env/matlab_desktop_icon.png" "${pkgdir}/usr/share/pixmaps/${pkgname}.png"
}

if [ ! -z ${_partialinstall+isSet} ] && [ -z ${_products+isSet} ]; then
  _products=(
	"Aerospace_Blockset"
	"Aerospace_Toolbox"
	"Antenna_Toolbox"
	"Audio_System_Toolbox"
	"Automated_Driving_System_Toolbox"
	"Bioinformatics_Toolbox"
	"Communications_System_Toolbox"
	"Computer_Vision_System_Toolbox"
	"Control_System_Toolbox"
	"Curve_Fitting_Toolbox"
	"Data_Acquisition_Toolbox"
	"Database_Toolbox"
	"Datafeed_Toolbox"
	"DO_Qualification_Kit"
	"DSP_System_Toolbox"
	"Econometrics_Toolbox"
	"Embedded_Coder"
	"Filter_Design_HDL_Coder"
	"Financial_Instruments_Toolbox"
	"Financial_Toolbox"
	"Fixed_Point_Designer"
	"Fuzzy_Logic_Toolbox"
	"Global_Optimization_Toolbox"
	"GPU_Coder"
	"HDL_Coder"
	"HDL_Verifier"
	"IEC_Certification_Kit"
	"Image_Acquisition_Toolbox"
	"Image_Processing_Toolbox"
	"Instrument_Control_Toolbox"
	"LTE_HDL_Toolbox"
	"LTE_System_Toolbox"
	"Mapping_Toolbox"
	"MATLAB"
	"MATLAB_Coder"
	"MATLAB_Compiler"
	"MATLAB_Compiler_SDK"
	"MATLAB_Distributed_Computing_Server"
	"MATLAB_Production_Server"
	"MATLAB_Report_Generator"
	"Model_Based_Calibration_Toolbox"
	"Model_Predictive_Control_Toolbox"
	"Neural_Network_Toolbox"
	"OPC_Toolbox"
	"Optimization_Toolbox"
	"Parallel_Computing_Toolbox"
	"Partial_Differential_Equation_Toolbox"
	"Phased_Array_System_Toolbox"
	"Polyspace_Bug_Finder"
	"Polyspace_Code_Prover"
	"Powertrain_Blockset"
	"Predictive_Maintenance_Toolbox"
	"RF_Blockset"
	"RF_Toolbox"
	"Risk_Management_Toolbox"
	"Robotics_System_Toolbox"
	"Robust_Control_Toolbox"
	"Signal_Processing_Toolbox"
	"SimBiology"
	"SimEvents"
	"Simscape"
	"Simscape_Driveline"
	"Simscape_Electronics"
	"Simscape_Fluids"
	"Simscape_Multibody"
	"Simscape_Power_Systems"
	"Simulink"
	"Simulink_3D_Animation"
	"Simulink_Check"
	"Simulink_Code_Inspector"
	"Simulink_Coder"
	"Simulink_Control_Design"
	"Simulink_Coverage"
	"Simulink_Design_Optimization"
	"Simulink_Design_Verifier"
	"Simulink_Desktop_Real_Time"
	"Simulink_PLC_Coder"
	"Simulink_Real_Time"
	"Simulink_Report_Generator"
	"Simulink_Requirements"
	"Simulink_Test"
	"Spreadsheet_Link"
	"Stateflow"
	"Statistics_and_Machine_Learning_Toolbox"
	"Symbolic_Math_Toolbox"
	"System_Identification_Toolbox"
	"Text_Analytics_Toolbox"
	"Trading_Toolbox"
	"Vehicle_Dynamics_Blockset"
	"Vehicle_Network_Toolbox"
	"Vision_HDL_Toolbox"
	"Wavelet_Toolbox"
	"WLAN_System_Toolbox"
  )
fi
