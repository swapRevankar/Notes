# Eye brow setup
- Create multiply node name L_main_rotate_multiplyDivide
- Base controller rotate Z to Multiply input1 X and Y
- PSD rotate_1 to Multiply input2 X and rotate_2 to Multiply input2 Y
- Create 3 add double linear node name them L_main_rotate_1_addDoubleLinear, L_main_rotate_2_addDoubleLinear, L_brow_sum_in_x_addDoubleLinear
- L_main_rotate_multiplyDivide.outputX to L_main_rotate_1_addDoubleLinear.input1
- L_brow_1_joint_ctlr.translateY to L_main_rotate_1_addDoubleLinear.input2
- L_main_rotate_multiplyDivide.outputY to L_main_rotate_2_addDoubleLinear.input1
- L_brow_in_joint_ctlr.translateY to L_main_rotate_2_addDoubleLinear.input2
- L_brow_1_joint_ctlr.translateX to L_brow_sum_in_x_addDoubleLinear.input1
- L_brow_in_joint_ctlr.translateX to L_brow_sum_in_x_addDoubleLinear.input2
- L_main_rotate_1_addDoubleLinear.output to L_brow_sum_plusMinusAverage.input3D[1].input3Dx
- L_brow_2_joint_ctlr.translateY to L_brow_sum_plusMinusAverage.input3D[1].input3Dy
- L_brow_3_joint_ctlr.translateY to L_brow_sum_plusMinusAverage.input3D[1].input3Dz
- L_brow_base_joint_ctlr.translateY to L_brow_sum_plusMinusAverage.input3D[0].input3Dxyz
- Create add double linear L_brow_sum_in_y_addDoubleLinear
- L_brow_sum_plusMinusAverage.output3D.output3Dx to L_brow_sum_in_y_addDoubleLinear.input1
- L_main_rotate_2_addDoubleLinear.output to L_brow_sum_in_y_addDoubleLinear.input2
- Create Multiply node L_brow_in_multiplyDivide
- L_brow_sum_in_y_addDoubleLinear.output to multiplyDivide1.input1.input1X
- L_brow_sum_in_x_addDoubleLinear.output to L_brow_in_multiplyDivide.input1.input1Y
- Add attribute to PSD named X and Y and set it to -10 10 
- brow_psd_data.X to L_brow_in_multiplyDivide.input2.input2X
- brow_psd_data.Y to L_brow_in_multiplyDivide.input2.input2Y
- L_brow_in_multiplyDivide.output.outputX to L_brow_in_locator.rotate.rotateX
- L_brow_in_multiplyDivide.output.outputY to L_brow_in_locator.rotate.rotateY


