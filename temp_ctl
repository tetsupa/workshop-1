/*----------------------------------------------------------
*【機能】 温度制御の状態管理を行なう *【引数】 温度制御の許可 *【戻り値】なし
*【補足】
*  周期ハンドラより起動される(500ms周期を想定)
 */
void TempControl(tTempCtl iTempCtl)
{
    tEventType event;
    event   = gEvent;
gEvent &= (~event); /* クリア */
    if (event & CHANGE_MODE) {
        sWarmMode = sWarmModeChangeTable[sWarmMode];
        Io_PotControl(POT_WARM_MODE, sWarmModeDispTable[sWarmMode]);
    }
    if (iTempCtl == TEMP_CTL_OFF) {
        gTempCtlState = IDLE;
    } else {
        if (event & REQUEST_BOIL) {
            gTempCtlState = BOILING;
} }
/* 水の状態を監視する */ checkWater();
/* ヒータ制御の管理 */
    switch (gTempCtlState) {
    case BOILING:
    case OVER_BOILING:
        heating(gTempCtlState);
        Io_PotControl(POT_HEAT_STATUS, 0x01);
        break;
    case KEEP_WARM:
        heating(gTempCtlState);
        Io_PotControl(POT_HEAT_STATUS, 0x02);
        break;
    case IDLE:
    case EMERGENCY:
sTargetHeatThrottle = 0;
{ /* ここで呼ぶとヒータが早く止まった 05.11.25 GOMA */
int mode = 0;
            DeviceWrite(REG_CONTROL_HEATERPOWER, 4, &mode);
        }
        Io_PotControl(POT_HEAT_STATUS, 0x00);
break; }
}
