# Eye brow setup
- Create Multiply node L_main_rotate_multiplyDivide
- connectAttr -f L_brow_base_joint_ctlr.rotateZ L_main_rotate_multiplyDivide.input1X
- connectAttr -f L_brow_base_joint_ctlr.rotateZ L_main_rotate_multiplyDivide.input1Y;
- Create 3 add double liner node and one mult double liner L_main_rotate_1_addDoubleLinear, L_brow_rotate_multDoubleLinear, L_brow_sum_in_x_addDoubleLinear, L_main_rotate_2_addDoubleLinear
- connectAttr -f L_main_rotate_multiplyDivide.outputX L_main_rotate_1_addDoubleLinear.input1
- connectAttr -f L_brow_1_joint_ctlr.translateY L_main_rotate_1_addDoubleLinear.input2
- connectAttr -f L_brow_1_joint_ctlr.rotateZ L_brow_rotate_multDoubleLinear.input1
- Create attribute on psd named rotateBrow
- connectAttr -f brow_psd_data.rotateBrow L_brow_rotate_multDoubleLinear.input2
- connectAttr -f L_brow_1_joint_ctlr.translateX L_brow_sum_in_x_addDoubleLinear.input1
- connectAttr -f L_brow_in_joint_ctlr.translateX L_brow_sum_in_x_addDoubleLinear.input2
- connectAttr -f L_main_rotate_multiplyDivide.outputY L_main_rotate_2_addDoubleLinear.input1
- connectAttr -f L_brow_in_joint_ctlr.translateY L_main_rotate_2_addDoubleLinear.input2
- Create Sum node L_brow_sum_plusMinusAverage
- connectAttr -f L_main_rotate_1_addDoubleLinear.output L_brow_sum_plusMinusAverage.input3D[1].input3Dx
- connectAttr -f L_brow_base_joint_ctlr.translateY L_brow_sum_plusMinusAverage.input3D[0].input3Dx;
-connectAttr -f L_brow_base_joint_ctlr.translateY L_brow_sum_plusMinusAverage.input3D[0].input3Dy;
- connectAttr -f L_brow_base_joint_ctlr.translateY L_brow_sum_plusMinusAverage.input3D[0].input3Dz;
- connectAttr -f L_brow_2_joint_ctlr.translateY L_brow_sum_plusMinusAverage.input3D[1].input3Dy;
- connectAttr -f L_brow_3_joint_ctlr.translateY L_brow_sum_plusMinusAverage.input3D[1].input3Dz;
- Create add double linear L_brow_rotate_addDoubleLinear
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dx L_brow_rotate_addDoubleLinear.input1;
- connectAttr -f L_brow_rotate_multDoubleLinear.output L_brow_rotate_addDoubleLinear.input2;
- Create add double linear L_brow_sum_in_y_addDoubleLinear
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dx L_brow_sum_in_y_addDoubleLinear.input1;
- connectAttr -f L_main_rotate_2_addDoubleLinear.output L_brow_sum_in_y_addDoubleLinear.input2;
- Create Multiply L_brow_2_multiplyDivide
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dy L_brow_2_multiplyDivide.input1X;
- Add attribute to PSD X and Y set to -10 10
- connectAttr -f brow_psd_data.X L_brow_2_multiplyDivide.input2X;
- connectAttr -f brow_psd_data.Y L_brow_2_multiplyDivide.input2Y;
- connectAttr -f L_brow_2_joint_ctlr.translateX L_brow_2_multiplyDivide.input1Y;
- Create multiply node L_brow_3_multiplyDivide
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dz L_brow_3_multiplyDivide.input1X;
- connectAttr -f brow_psd_data.X L_brow_3_multiplyDivide.input2X;
- connectAttr -f brow_psd_data.Y L_brow_3_multiplyDivide.input2Y;
- connectAttr -f L_brow_3_joint_ctlr.translateX L_brow_3_multiplyDivide.input1Y;
- Create Multiply L_brow_1_multiplyDivide
- connectAttr -f L_brow_rotate_addDoubleLinear.output L_brow_1_multiplyDivide.input1X;
- connectAttr -f L_brow_1_joint_ctlr.translateX L_brow_1_multiplyDivide.input1Y;
- connectAttr -f brow_psd_data.X L_brow_1_multiplyDivide.input2X;
- connectAttr -f brow_psd_data.Y L_brow_1_multiplyDivide.input2Y;
- connectAttr -f L_brow_2_multiplyDivide.outputX L_brow_2_locator.rotateX;
- connectAttr -f L_brow_2_multiplyDivide.outputY L_brow_2_locator.rotateY;
- connectAttr -f L_brow_3_multiplyDivide.outputX L_brow_3_locator.rotateX;
- connectAttr -f L_brow_3_multiplyDivide.outputY L_brow_3_locator.rotateY;
- Create Multiply L_brow_in_multiplyDivide
- connectAttr -f brow_psd_data.X L_brow_in_multiplyDivide.input2X;
- connectAttr -f brow_psd_data.Y L_brow_in_multiplyDivide.input2Y;
- connectAttr -f L_brow_sum_in_y_addDoubleLinear.output L_brow_in_multiplyDivide.input1X;
- connectAttr -f L_brow_sum_in_x_addDoubleLinear.output L_brow_in_multiplyDivide.input1Y;
- connectAttr -f L_brow_1_multiplyDivide.outputX L_brow_1_locator.rotateX;
- connectAttr -f L_brow_1_multiplyDivide.outputY L_brow_1_locator.rotateY;
- connectAttr -f L_brow_in_multiplyDivide.outputX L_brow_in_locator.rotateX;
- connectAttr -f L_brow_in_multiplyDivide.outputY L_brow_in_locator.rotateY;

# Bring brows forward
- Create setrange L_brow_up_3_psd_setRange min 0 max 1
- Add attributes to PSD brow_up_3_TZ,brow_up_3_TY,brow_up_3_RX values (-0.58,1.22,0)
- connectAttr -f brow_psd_data.brow_up_3_TZ L_brow_up_3_psd_setRange.maxX;
- connectAttr -f brow_psd_data.brow_up_3_TY L_brow_up_3_psd_setRange.maxY;
- connectAttr -f brow_psd_data.brow_up_3_RX L_brow_up_3_psd_setRange.maxZ;
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dz L_brow_up_3_psd_setRange.valueX;
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dz L_brow_up_3_psd_setRange.valueY;
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dz L_brow_up_3_psd_setRange.valueZ;
- Set L_brow_low_3_psd_setRange Old Min X Y Z to -1 max 0
- Create setrange L_brow_up_2_psd_setRange min 0 max 1
- Old min to 0
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dy L_brow_up_2_psd_setRange.valueX;
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dy L_brow_up_2_psd_setRange.valueY;
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dy L_brow_up_2_psd_setRange.valueZ;
- Add attributes to PSD brow_up_2_TZ,brow_up_2_TY,brow_up_2_RX(-0.58,1.22,0)
- connectAttr -f brow_psd_data.brow_up_2_TZ L_brow_up_2_psd_setRange.maxX;
- connectAttr -f brow_psd_data.brow_up_2_TY L_brow_up_2_psd_setRange.maxY;
- connectAttr -f brow_psd_data.brow_up_2_RX L_brow_up_2_psd_setRange.maxZ;
- Create setrange L_brow_low_3_psd_setRange set old min to -1
- Add attributes to PSD brow_low_3_TZ,brow_low_3_TY,brow_low_3_RX(0.290,-1.64,0)
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dz L_brow_low_3_psd_setRange.valueX;
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dz L_brow_low_3_psd_setRange.valueY;
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dz L_brow_low_3_psd_setRange.valueZ;
- connectAttr -f brow_psd_data.brow_low_3_TZ L_brow_low_3_psd_setRange.minX;
- connectAttr -f brow_psd_data.brow_low_3_TY L_brow_low_3_psd_setRange.minY;
- connectAttr -f brow_psd_data.brow_low_3_RX L_brow_low_3_psd_setRange.minZ;
- Create setRange L_brow_low_2_psd_setRange old min to -1 max 0
- Add attributes to PSD brow_low_2_TZ,brow_low_2_TY, brow_low_2_RX(0.29,-1.64,0)
- connectAttr -f brow_psd_data.brow_low_2_TZ L_brow_low_2_psd_setRange.minX;
- connectAttr -f brow_psd_data.brow_low_2_TY L_brow_low_2_psd_setRange.minY;
- connectAttr -f brow_psd_data.brow_low_2_RX L_brow_low_2_psd_setRange.minZ;
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dy L_brow_low_2_psd_setRange.valueX;
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dy L_brow_low_2_psd_setRange.valueY;
- connectAttr -f L_brow_sum_plusMinusAverage.output3Dy L_brow_low_2_psd_setRange.valueZ;
- Create setrange L_brow_low_In_psd_setRange old min -1 max 0
- Add attributes to PSD brow_low_in_TZ,brow_low_in_TY,brow_low_in_RX(0.29,-1.64,0)
- connectAttr -f brow_psd_data.brow_low_in_TZ L_brow_low_In_psd_setRange.minX;
- connectAttr -f brow_psd_data.brow_low_in_TY L_brow_low_In_psd_setRange.minY;
- connectAttr -f brow_psd_data.brow_low_in_RX L_brow_low_In_psd_setRange.minZ;
- connectAttr -f L_brow_sum_in_y_addDoubleLinear.output L_brow_low_In_psd_setRange.valueX;
- connectAttr -f L_brow_sum_in_y_addDoubleLinear.output L_brow_low_In_psd_setRange.valueY;
- connectAttr -f L_brow_sum_in_y_addDoubleLinear.output L_brow_low_In_psd_setRange.valueZ;
- Create setrange L_brow_up_In_psd_setRange old min 0 max 1
- Add attributes to PSD brow_up_in_TZ,brow_up_in_TY,brow_up_in_RX(-0.58,1.22,0)
- connectAttr -f brow_psd_data.brow_up_in_TZ L_brow_up_In_psd_setRange.maxX;
- connectAttr -f brow_psd_data.brow_up_in_TY L_brow_up_In_psd_setRange.maxY;
- connectAttr -f brow_psd_data.brow_up_in_RX L_brow_up_In_psd_setRange.maxZ;
- Create plus L_brow_3_plusMinusAverage
- connectAttr -f L_brow_up_3_psd_setRange.outValue L_brow_3_plusMinusAverage.input3D[0];
- connectAttr -f L_brow_low_3_psd_setRange.outValue L_brow_3_plusMinusAverage.input3D[1];
- Create setrange L_brow_up_1_psd_setRange old min 0 max 1
- Add attributes to PSD brow_up_1_TZ,brow_up_1_TY,brow_up_1_RX(-0.58,1.22,0)
- connectAttr -f brow_psd_data.brow_up_1_TZ L_brow_up_1_psd_setRange.maxX;
- connectAttr -f brow_psd_data.brow_up_1_TY L_brow_up_1_psd_setRange.maxY;
- connectAttr -f brow_psd_data.brow_up_1_RX L_brow_up_1_psd_setRange.maxZ;
- connectAttr -f L_brow_rotate_addDoubleLinear.output L_brow_up_1_psd_setRange.valueX;
- connectAttr -f L_brow_rotate_addDoubleLinear.output L_brow_up_1_psd_setRange.valueY;
- connectAttr -f L_brow_rotate_addDoubleLinear.output L_brow_up_1_psd_setRange.valueZ;
- Create plus L_brow_2_plusMinusAverage
- connectAttr -f L_brow_up_2_psd_setRange.outValue L_brow_2_plusMinusAverage.input3D[0];
- connectAttr -f L_brow_low_2_psd_setRange.outValue L_brow_2_plusMinusAverage.input3D[1];
- Create setrange L_brow_low_1_psd_setRange old min -1 max 0
- Add attributes to PSD brow_low_1_TZ,brow_low_1_TY,brow_low_1_RX(0.29,-1.64,0)
- connectAttr -f brow_psd_data.brow_low_1_TZ L_brow_low_1_psd_setRange.minX;
- connectAttr -f brow_psd_data.brow_low_1_TY L_brow_low_1_psd_setRange.minY;
- connectAttr -f brow_psd_data.brow_low_1_RX L_brow_low_1_psd_setRange.minZ;
- connectAttr -f L_brow_rotate_addDoubleLinear.output L_brow_low_1_psd_setRange.valueX;
- connectAttr -f L_brow_rotate_addDoubleLinear.output L_brow_low_1_psd_setRange.valueY;
- connectAttr -f L_brow_rotate_addDoubleLinear.output L_brow_low_1_psd_setRange.valueZ;
- Create plus L_brow_In_plusMinusAverage
- connectAttr -f L_brow_low_In_psd_setRange.outValue L_brow_In_plusMinusAverage.input3D[1];
- connectAttr -f L_brow_up_In_psd_setRange.outValue L_brow_In_plusMinusAverage.input3D[0];
- connectAttr -f L_brow_3_plusMinusAverage.output3Dx L_brow_3_joint.translateZ;
- connectAttr -f L_brow_3_plusMinusAverage.output3Dy L_brow_3_joint.translateY;
- connectAttr -f L_brow_3_plusMinusAverage.output3Dz L_brow_3_joint.rotateX;
- connectAttr -f L_brow_2_plusMinusAverage.output3Dx L_brow_2_joint.translateZ;
- connectAttr -f L_brow_2_plusMinusAverage.output3Dy L_brow_2_joint.translateY;
- connectAttr -f L_brow_2_plusMinusAverage.output3Dz L_brow_2_joint.rotateX;
- Create plus L_brow_1_plusMinusAverage
- connectAttr -f L_brow_up_1_psd_setRange.outValue L_brow_1_plusMinusAverage.input3D[0];
- connectAttr -f L_brow_low_1_psd_setRange.outValue L_brow_1_plusMinusAverage.input3D[1];
- connectAttr -f L_brow_In_plusMinusAverage.output3Dx L_brow_in_joint.translateZ;
- connectAttr -f L_brow_In_plusMinusAverage.output3Dy L_brow_in_joint.translateY;
- connectAttr -f L_brow_In_plusMinusAverage.output3Dz L_brow_in_joint.rotateX;
- connectAttr -f L_brow_1_plusMinusAverage.output3Dx L_brow_1_joint.translateZ;
- connectAttr -f L_brow_1_plusMinusAverage.output3Dy L_brow_1_joint.translateY;
- connectAttr -f L_brow_1_plusMinusAverage.output3Dz L_brow_1_joint.rotateX;










 





