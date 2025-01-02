- 👋 Hi, I’m @Gowtham-wma
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
Gowtham-wma/Gowtham-wma is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
     string sSql = "select tp_limit_tested from tp_data where rownum = 1";
                                m_serviceAgent.iExecuteCommand(oWMACVMBase.m_iCurrContext, sSql,
                                    (iRowsAffected, ex) =>
                                    {
                                        string sTemp1 = "";
                                        string sTemp2 = "";
                                        string sTemp3 = "";
                                        bHaveTpLimitTested = false;
                                        if (iRowsAffected != 0)
                                        {
                                            bHaveTpLimitTested = true;
                                            sTemp1 = " ,TPD.TP_LIMIT_TESTED";
                                            sTemp2 = " ,CASE WHEN T2.TP_LIMIT_TESTED IS NULL THEN 'NA' ELSE T2.TP_LIMIT_TESTED END AS TP_LIMIT_WAS_TESTED";
                                            sTemp3 = ", T3.TP_LIMIT_WAS_TESTED AS sTpLimitTested";
                                            ex = null;
                                        }

                                        #region code
                                        #region Sql
                                        sSql =
                                            " SELECT T3.THREAD_NUM as lThreadNum, T3.PHASE_ID as lPhaseId, T3.PHASE_TEXT as sPhase, T3.STEP_ID as lStepId, T3.STEP_TEXT as sStep, T3.TP_ID as lId, T3.TP_TEXT as sText, " +
                                            "  TO_NUMBER(TO_CHAR(FN_GET_LIMIT(PD.MAX_PARAM_VAL_REV_DTS, case PTPDL.PARAM_CODE when 'M' then PD.MODEL_CODE when 'S' then PD.STAND_CODE end, PTPDL.PARAM_CODE, PTPDL.PARAM_ID),CONCAT('9999999999',T3.FORMAT_SPECIFIER_TEXT))) dLowLimit, " +
                                            "  TO_NUMBER(TO_CHAR(T3.TP_QTY,CONCAT('9999999999',T3.FORMAT_SPECIFIER_TEXT))) dValue, " +
                                            "  TO_NUMBER(TO_CHAR(FN_GET_LIMIT(PD.MAX_PARAM_VAL_REV_DTS, case PTPDH.PARAM_CODE when 'M' then PD.MODEL_CODE when 'S' then PD.STAND_CODE end, PTPDH.PARAM_CODE, PTPDH.PARAM_ID),CONCAT('9999999999',T3.FORMAT_SPECIFIER_TEXT))) dHighLimit, " +
                                            "  TO_CHAR(FN_GET_LIMIT(PD.MAX_PARAM_VAL_REV_DTS, case PTPDL.PARAM_CODE when 'M' then PD.MODEL_CODE when 'S' then PD.STAND_CODE end, PTPDL.PARAM_CODE, PTPDL.PARAM_ID),CONCAT('9999999999',T3.FORMAT_SPECIFIER_TEXT)) sFormattedLowLimit, " +
                                            "  TO_CHAR(T3.TP_QTY,CONCAT('9999999999',T3.FORMAT_SPECIFIER_TEXT)) sFormattedValue, " +
                                            "  TO_CHAR(FN_GET_LIMIT(PD.MAX_PARAM_VAL_REV_DTS, case PTPDH.PARAM_CODE when 'M' then PD.MODEL_CODE when 'S' then PD.STAND_CODE end, PTPDH.PARAM_CODE, PTPDH.PARAM_ID),CONCAT('9999999999',T3.FORMAT_SPECIFIER_TEXT)) sFormattedHighLimit, " +
                                            "  UT.UNITS_TEXT as sUnits, T3.PHASE_ORDER_NUM as lPhaseOrderNum, T3.STEP_ORDER_NUM as lStepOrderNum" +
                                            " , PD.TEST_DTS as dtTestTime, PD.STAND_CODE as sStandCode, T3.TP_DEFN_SAKEY as lTpDefnSakey, PTPDL.PARAM_ID as lLowLimitId, PTPDH.PARAM_ID as lHighLimitId, PD.FAMILY_ID as sFamilyId, T3.TP_CRITICAL_FLAG as sTpCritical " +
                                                sTemp3 +
                                            " FROM " +
                                    #region T3
                                            " ( SELECT " +
                                            "		T2. *, " +
                                            "       CASE WHEN PSI.THREAD_NUM IS NULL THEN TO_NUMBER('9') ELSE PSI.THREAD_NUM END AS THREAD_NUM, " +
                                            "       CASE WHEN PSI.PHASE_ORDER_NUM IS NULL THEN TO_NUMBER('99999') ELSE PSI.PHASE_ORDER_NUM END AS PHASE_ORDER_NUM, " +
                                            "       CASE WHEN PSI.STEP_ORDER_NUM IS NULL THEN TO_NUMBER('99999') ELSE PSI.STEP_ORDER_NUM END AS STEP_ORDER_NUM, " +
                                            "       CASE WHEN PSI.PHASE_ID IS NULL THEN T2.PHASE_ID_DIS ELSE PSI.PHASE_ID END AS PHASE_ID, " +
                                            "       CASE WHEN PSI.STEP_ID IS NULL THEN T2.STEP_ID_DIS ELSE PSI.STEP_ID END AS STEP_ID, " +
                                            "       CASE WHEN PSI.PHASE_TEXT IS NULL THEN T2.PHASE_TEXT_DIS ELSE PSI.PHASE_TEXT END AS PHASE_TEXT, " +
                                            "       CASE WHEN PSI.STEP_TEXT IS NULL THEN T2.STEP_TEXT_DIS ELSE PSI.STEP_TEXT END AS STEP_TEXT, " +
                                            "       CASE WHEN T2.TP_ACTIVE_FLAG = 'C' THEN 'Y' ELSE 'N' END AS TP_CRITICAL_FLAG " +
                                                    sTemp2 +
                                            "   FROM " +
                                    #region T2
                                            " 	    ( SELECT T2A.*, T2B.* " +
                                            "  	      FROM " +
                                            "	    		(SELECT  " +
                                            "	    	    	TPD.TP_DEFN_SAKEY, TPD.TP_QTY, TPT.TEST_TEXT PHASE_TEXT_DIS, TPT.TEST_TEXT_ID PHASE_ID_DIS, " +
                                            "			    	TPDEF.PHASE_PHRASE_SAKEY, TPDEF.TP_ID, TPDEF.TP_TEXT, TPDEF.UNITS_TEXT_SAKEY, TPDEF.FORMAT_SPECIFIER_TEXT, TPDEF.TP_ACTIVE_FLAG " +
                                                                sTemp1 +
                                            "				FROM TP_DATA TPD, TP_DEFN TPDEF, TEST_TEXT TPT " +
                                            "				WHERE TPD.TEST_DTS = to_date('{0}' , 'mm/dd/yyyy hh24:mi:ss') AND TPD.STAND_CODE = '{1}'" +
                                            "               	AND TPD.TP_DEFN_SAKEY = TPDEF.TP_DEFN_SAKEY AND TPT.TEST_TEXT_SAKEY(+) = TPDEF.PHASE_PHRASE_SAKEY " +
                                            "				) T2A," +
                                            "	    		(SELECT  " +
                                            "	    	    	TPDEF.TP_ID MY_ID, TST.TEST_TEXT STEP_TEXT_DIS, TST.TEST_TEXT_ID STEP_ID_DIS, TPDEF.STEP_PHRASE_SAKEY " +
                                            "				FROM TP_DATA TPD, TP_DEFN TPDEF, TEST_TEXT TST " +
                                            "				WHERE TPD.TEST_DTS = to_date('{0}' , 'mm/dd/yyyy hh24:mi:ss') AND TPD.STAND_CODE = '{1}'" +
                                            "               	AND TPD.TP_DEFN_SAKEY = TPDEF.TP_DEFN_SAKEY AND TST.TEST_TEXT_SAKEY(+) = TPDEF.STEP_PHRASE_SAKEY " +
                                            "				) T2B	" +
                                            "  		  WHERE " +
                                            "				T2A.TP_ID = T2B.MY_ID " +
                                            "		) T2,  " +
                                    #endregion T2
                                    #region PSI
                                            "	    ( SELECT " +
                                            "		   	PSO.THREAD_NUM, MAX(PSO.PHASE_ORDER_NUM) PHASE_ORDER_NUM, MAX(PSO.STEP_ORDER_NUM) STEP_ORDER_NUM, PSO.PHASE_PHRASE_SAKEY, PSO.STEP_PHRASE_SAKEY, " +
                                            "		    TPT.TEST_TEXT PHASE_TEXT, TPT.TEST_TEXT_ID PHASE_ID, " +
                                            "           TST.TEST_TEXT STEP_TEXT, TST.TEST_TEXT_ID STEP_ID  " +
                                            "       FROM" +
                                            "           PHASE_STEP_ORDER PSO, TEST_TEXT TPT, TEST_TEXT TST " +
                                            "       WHERE " +
                                            "	        PSO.TEST_DTS(+) = to_date('{0}' , 'mm/dd/yyyy hh24:mi:ss') AND PSO.STAND_CODE(+) = '{1}' AND " +
                                            "	        TPT.TEST_TEXT_SAKEY(+) = PSO.PHASE_PHRASE_SAKEY AND TST.TEST_TEXT_SAKEY(+) = PSO.STEP_PHRASE_SAKEY " +
                                            "       GROUP BY PSO.THREAD_NUM, PSO.PHASE_PHRASE_SAKEY, PSO.STEP_PHRASE_SAKEY, TPT.TEST_TEXT, TPT.TEST_TEXT_ID,TST.TEST_TEXT, TST.TEST_TEXT_ID " +
                                            "       ) PSI " +
                                    #endregion PSI
                                            "    WHERE " +
                                            "		T2.PHASE_PHRASE_SAKEY = PSI.PHASE_PHRASE_SAKEY(+) AND T2.STEP_PHRASE_SAKEY = PSI.STEP_PHRASE_SAKEY(+)  " +
                                            " ) T3," +
                                    #endregion T3
                                            " UNITS_TEXT UT, " +
                                            " PARAM_TP_DEFN PTPDL, " +
                                            " PARAM_TP_DEFN PTPDH, " +
                                    #region PD
                                            " ( SELECT " +
                                            "       PD.TEST_DTS, PD.STAND_CODE, PD.MODEL_CODE, PD.MAX_PARAM_DEFN_REV_DTS, PD.MAX_PARAM_VAL_REV_DTS, PD.FAMILY_ID " +
                                            "   FROM " +
                                            "       PRODUCT_DATA PD " +
                                            "   WHERE " +
                                            "		PD.TEST_DTS = to_date('{0}' , 'mm/dd/yyyy hh24:mi:ss') AND PD.STAND_CODE = '{1}' " +
                                            " )PD " +
                                    #endregion PD
                                            " WHERE T3.UNITS_TEXT_SAKEY = UT.UNITS_TEXT_SAKEY AND " +
                                            "		PTPDL.TP_DEFN_SAKEY(+) = T3.TP_DEFN_SAKEY AND PTPDL.LIMIT_CODE(+) = 'L' AND " +
                                            "		PTPDH.TP_DEFN_SAKEY(+) = T3.TP_DEFN_SAKEY AND PTPDH.LIMIT_CODE(+) = 'H' " +
                                            " ORDER BY " +
                                            "       THREAD_NUM, PHASE_ORDER_NUM, STEP_ORDER_NUM";
                                        #endregion Sql
