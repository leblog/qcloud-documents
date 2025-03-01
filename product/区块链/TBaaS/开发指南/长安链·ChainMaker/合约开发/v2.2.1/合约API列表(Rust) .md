ChainMake Rust 语言版本智能合约有丰富的 API 接口，供用户在撰写智能合约的时候与链进行交互，代码实现详情可以参考[API 接口代码实现](https://docs.chainmaker.org.cn/v2.2.1/html/operation/%E6%99%BA%E8%83%BD%E5%90%88%E7%BA%A6.html#rust)。

从逻辑方面划分，可将 API 划分为以下类型：

[](id:informationExtraction)

### 交易信息提取

<table><thead>
<tr>
<th width="50%">接口</th>
<th>说明</th>
</tr>
</thead>
<tbody><tr>
<td>get_creator_org_id(&self) -> String </td>
<td>获取合约创建者所属组织 ID</td>
</tr>
<tr>
<td>get_creator_role(&self) -> String </td>
<td>获取合约创建者角色</td>
</tr>
<tr>
<td>get_creator_pub_key(&self) -> String</td>
<td>获取合约创建者公钥</td>
</tr>
<tr>
<td>get_sender_org_id(&self) -> String </td>
<td>获取交易发起者所属组织 ID</td>
</tr>
<tr>
<td>get_sender_role(&self) -> String </td>
<td>获取交易发起者角色</td>
</tr>
<tr>
<td>get_tx_id(&self) -> String</td>
<td>获取交易 ID</td>
</tr>
<tr>
<td>get_sender_pub_key(&self) -> String  </td>
<td>获取交易发起者公钥</td>
</tr>
<tr>
<td>get_block_height(&self) -> u64 </td>
<td>获取当前区块高度</td>
</tr>
</tbody></table>

[](id:accountInteraction)

### 账本交互

<table><thead>
<tr>
<th width="60%">接口</th>
<th>说明</th>
</tr>
</thead>
<tbody><tr>
<td>get_state(&self, key: &str, field: &str) -> Result&lt;Vec&lt;u8&gt;, result_code&gt;</td>
<td>获取合约账户信息。该接口可从链上获取类别 “key” 下属性名为 “field” 的状态信息。</td>
</tr>
<tr>
<td>get_state_from_key(&self, key: &str) -> Result&lt;Vec&lt;u8&gt;, result_code&gt;</td>
<td>获取合约账户信息。该接口可以从链上获取类别为 key 的状态信息</td>
</tr>
<tr>
<td>put_state(&self, key: &str, field: &str, value: &[u8]) -> result_code</td>
<td>写入合约账户信息。该接口可把类别 “key” 下属性名为 “filed” 的状态更新到链上。更新成功返回 0，失败则返回 1。</td>
</tr>
<tr>
<td>put_state_from_key(&self, key: &str, value: &[u8]) -> result_code</td>
<td>写入合约账户信息。</td>
</tr>
<tr>
<td>delete_state(&self, key: &str, field: &str) -> result_code</td>
<td>删除合约账户信息。该接口可把类别 “key” 下属性名为 “name” 的状态从链上删除。</td>
</tr>
<tr>
<td>delete_state_from_key(&self, key: &str) -> result_code;</td>
<td>删除合约账户信息。该接口可把类别 “key” 下属性名为 “name” 的状态从链上删除。</td>
</tr>
<tr>
<td>call_contract(&self, contract_name: &str, method: &str, param: EasyCodec) -> Result&lt;Vec&lt;u8&gt;, result_code&gt;;</td>
<td>跨合约调用。</td>
</tr>

</tbody></table>

[](id:parametersProcess)

### 参数处理

<table>
<thead>
<tr>
<th width="50%">接口</th>
<th>说明</th>
</tr>
</thead>
<tbody><tr>
<td>args(&self) -> &EasyCodec</td>
<td>该接口调用 getArgsMap() 接口，把 json 格式的数据反序列化，并将解析出的数据返还给用户。</td>
</tr>
<tr>
<td>arg(&self, key: &str) -> Result&lt;Vec&lt;u8&gt;, String&gt;</td>
<td>该接口可返回属性名为 “key” 的参数的属性值（输出格式为Result&lt;Vec&lt;u8&gt;, String&gt;）。</td>
</tr>
<tr>
<td>arg_as_utf8_str(&self, key: &str) -> String;</td>
<td>该接口可返回属性名为 “key” 的参数的属性值（输出格式为String）。</td>
</tr>
</tbody></table>

[](id:otherClass)

### 其他辅助类

<table>
<thead>
<tr>
<th width="65%">接口</th>
<th>说明</th>
</tr>
</thead>
<tbody><tr>
<td>ok(&self, value: &[u8]) -> result_code</td>
<td>该接口可记录用户操作成功的信息，并将操作结果记录到链上。</td>
</tr>
<tr>
<td>error(&self, body: &str) -> result_code</td>
<td>该接口可记录用户操作失败的信息，并将操作结果记录到链上。</td>
</tr>
<tr>
<td>log(&self, msg: &str)</td>
<td>该接口可记录事件日志。查看方式为在链配置的 log.yml 中，开启 vm:debug 即可看到类似：gasm log>> + msg</td>
</tr>
<tr>
<td>emit_event(&mut self, topic: &str, data: &Vec&lt;String&gt;) -> result_code</td>
<td>发送合约事件</td>
</tr>
<tr>
<td>new_iterator(&self,start_key: &str,limit_key: &str) -> Result&lt;Box&lt;dyn ResultSet&gt;, result_code&gt;</td>
<td>新建 key 范围迭代器，key 前闭后开，即：startKey <= dbkey < limitKey</td>
</tr>
<tr>
<td>new_iterator_with_field(&self, key: &str, start_field: &str, limit_field: &str) -> Result&lt;Box&lt;dyn ResultSet&gt;, result_code&gt;</td>
<td>新建 field 范围迭代器，key 需相同，field 前闭后开，即：key = dbdbkey and startField <= dbfield < limitField</td>
</tr>
<tr>
<td>new_iterator_prefix_with_key(&self, key: &str) -> Result&lt;Box&lt;dyn ResultSet&gt;, result_code&gt;</td>
<td>新建指定 key 前缀匹配迭代器，key 需前缀一致，即 dbkey.startWith(key)</td>
</tr>
<tr>
<td>new_iterator_prefix_with_key_field(&self, key: &str, field: &str) -> Result&lt;Box&lt;dyn ResultSet&gt;, result_code&gt;</td>
<td>新建指定 field 前缀匹配迭代器，key 需相同，field 前缀一致，即 dbkey = key and dbfield.startWith(field)</td>
</tr>
</tbody></table>
