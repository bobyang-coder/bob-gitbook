### Class文件中的常量池 {#hh呵呵}

> 在Class文件结构中，最头的4个字节用于存储魔数Magic Number，用于确定一个文件是否能被JVM接受，再接着4个字节用于存储版本号，前2个字节存储次版本号，后2个存储主版本号，再接着是用于存放常量的常量池，由于常量的数量是不固定的，所以常量池的入口放置一个U2类型的数据\(constant\_pool\_count\)存储常量池容量计数值。
>
> 常量池主要用于存放两大类常量：字面量\(Literal\)和符号引用量\(Symbolic References\)，
>
> 字面量相当于Java语言层面常量的概念，如文本字符串，声明为final的常量值等，符号引用则属于编译原理方面的概念，包括了如下三种类型的常量：



![](/assets/11.png)

```java
@Override
public void update(BindCard bindCard) {
    bindCardService.updateBindCard(bindCard);
}

/**
 * 修改卡信息接口
 *
 * @param bindCardReq
 * @param type        baseMsg,phoneNo
 * @return
 * @throws ServiceException
 */
public BindCard midifyCardMsg(BindCardReq bindCardReq, String type) throws ServiceException {
    logger.info("【修改卡信息接口】bindCardReq=" + bindCardReq.toString());
    this.validateParam(bindCardReq, type);
    this.validateParamLength(bindCardReq, null);
    String memberNo = bindCardReq.getMemberNo();
    memberCheckService.checkMemberStatus(bindCardReq.getCustomerNo(), bindCardReq.getMemberNo(), true);
    Object member = memberCheckService.checkMemeberExist(bindCardReq.getCustomerNo(), memberNo);
    if (member instanceof CustEnterprise) {
        throw new ServiceException(BizCode.BIND_CARD_NOT_SUPPORT_ENTERPRISE_MEMBER.getCode(), BizCode.BIND_CARD_NOT_SUPPORT_ENTERPRISE_MEMBER.getMsg());
    }
    //判断绑卡是否存在
    BindCard bindCard = bindCardService.getBindCardById(bindCardReq.getBindId());//查询绑卡信息
    if (null == bindCard || !CustmemConstants.BIND_STATUS_BINDED.equals(bindCard.getBindStatus())) {
        throw new ServiceException(BizCode.CARD_MSG_NOT_EXIST.getCode(), BizCode.CARD_MSG_NOT_EXIST.getMsg());
    }
    if (!bindCard.getMemberNo().equals(bindCardReq.getMemberNo()) || !bindCard.getMerchantId().equals(bindCardReq.getCustomerNo())) {//判断绑卡id是否属于该会员下的
        throw new ServiceException(BizCode.CARD_MSG_NOT_EXIST.getCode(), BizCode.CARD_MSG_NOT_EXIST.getMsg());
    }
    //以下赋值只是为了传参给支付密码页面
    bindCard.setProvince(bindCardReq.getProvince());
    bindCard.setCity(bindCardReq.getCity());
    bindCard.setBranchBank(bindCardReq.getBranchBank());
    bindCard.setSubBranchBank(bindCardReq.getSubBranchBank());
    bindCard.setMobile(bindCardReq.getMobile());
    return bindCard;
}
```

 

 

 

---

创建时间：2017年05月13日09:03:54

