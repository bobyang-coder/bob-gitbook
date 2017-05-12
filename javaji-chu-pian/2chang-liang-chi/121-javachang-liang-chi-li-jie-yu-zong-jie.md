```java
/**
     * 会员绑卡接口
     *
     * @param merchantId
     * @param data
     * @param encryptKey
     * @param version
     * @param interfaceType 接口类型
     * @param sendPvcFlag   是否发送验证码
     * @return
     * @throws Exception
     */
    public Map<String, Object> bind(String merchantId, String data, String encryptKey, String version, String interfaceType, boolean sendPvcFlag) {
        Map<String, Object> map = new HashMap<String, Object>(2);
        BindCardRes bindCardRes = new BindCardRes();
        try {
            BindCardReq bindCardReq = commonBusiness.decryptReqDataAndCheckSign(merchantId, data, encryptKey, version, BindCardReq.class);
            map.put("bindCardReq", bindCardReq);
            bindCardRes.setMemberNo(bindCardReq.getMemberNo());
            bindCardRes.setDoneTime(new Date());
            bindCardRes.setCharset(bindCardReq.getCharset());
            BindCard bindCard = bindCardBizService.bindCard(bindCardReq, interfaceType);
            if (sendPvcFlag) {
                if (CustmemConstants.INTERFACE_TYPE_SDK.equals(interfaceType)) {//SDK绑卡校验通过自动触发发送短信
                    commonBusiness.sendPhoneVCode(bindCardReq.getMobile(), CustmemConstants.VCODE_CHANNAL_BINDCARD_SUBMIT, "M1038");
                } else {
                    commonBusiness.sendPhoneVCode(bindCardReq.getMobile(), CustmemConstants.VCODE_CHANNAL_MEMCLI_BINDCARD, "M1038");
                }
            }
            bindCardRes.setBindId(bindCard.getBindId());
            bindCardRes.setBankName(bindCard.getBankName());
            bindCardRes.setCardType(bindCard.getCardType());
            bindCardRes.setResultCode(BizCode.SYSTEM_SUCCESS.getCode());
            bindCardRes.setResultMsg(BizCode.SYSTEM_SUCCESS.getMsg());
        } catch (ServiceException e) {
            logger.warn("[会员绑卡-ServiceException] - code:{},msg:{}", e.getCode(), e.getMessage(), e);
            bindCardRes.setResultCode(e.getCode());
            bindCardRes.setResultMsg(e.getMessage());
        } catch (Exception e) {
            logger.error("[RBException] - [bind] - errorMsg:[会员绑卡异常{}]", e.getMessage(), e);
            bindCardRes.setResultCode(BizCode.SYSTEM_ERROR.getCode());
            bindCardRes.setResultMsg(BizCode.SYSTEM_ERROR.getMsg());
        }
        logger.info("[会员绑卡返回密文] - resultenData:{}", JSON.toJSONString(bindCardRes));
        map.put("bindCardRes", bindCardRes);
        return map;
    }
```



