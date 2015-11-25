	 Crypto核心包含所有的crypto涉及的方法都被toxcore使用。涉及到随机数,加密,解密, 关键字的生成,随机数,随机随机数。当然不是所有的crypto库方法都被打包了,只有那些需要的被打包了。 例如, NaCl 方法要求在数据前面有一个零字节的缓冲区。你将会看到crypto库提供的方法使用在 toxcore代码里面,不仅仅只是crypto_core 方法。

	创建和处理请求函数是加密和解密的功能,用来一种类型的DHT包直接对其他的DHT节点发送数据,说实话,他们应该在DHT模块里面,但是看起来他们更适合这里。
	
	这个模块的目的是对一些 crypto 涉及到的功能提供友好的接口。

####TODO: put this in the intro.

	在toxcore中,所有的公钥都是32字节 , 并且是在NaCl crypto库里面的crypto_box_keypair() 函数生成。

	NaCl crypto库的crypto_box*()方法用来对所有的包进行加密,解密。

	像在NaCl文档中解释的一样,crypto_box是用Curve25519生成一个共享的加密密钥,通过将会接收到的包的Curve25519公钥和发送包的Curve25519私钥,进行公钥加密。

	当接收者接收到包的时候,他们将会解析两者一对一的密钥生成共享密钥 (接收者私钥 一一对应的接收者的公钥  发送者的公钥和一一对应的发送者的公私钥)用来生成共享密钥的对方的密钥,用函数/算法 加密包 , 得到相同的共享密钥。

	32字节的共享密钥用来对包加密,解密。 事实上必须考虑,因为它意味着任何一个可用包的加密和解密, 被用来 加密或者解密包的密钥 ,都是相同的。它也意味着包被发送可能返回给发送者,如果没有任何事情阻止它。

	共享密钥的生成是 加密/解密最敏感资源部分,  意味着通过保存共享密钥 ,尽可能的重用它们, 资源运用可以被减少  

	一个共享密钥生成,可以使用xslasa20加密,poly1305 验证, 这就是认证对称密钥加密。
	
	crypto_box 的随机数是24字节。

	用随机随机数的生成函数生成的随机数,在toxcore中无处不在。在toxcore中使用了一个被NaCl曝光的密码学安全随机数生成器,预防这个新产生的随机数和前一个随机数有联系而导致问题,像onion module.如果不同的包基于随机数是用rand生成的可以绑在一起，例如 它可能会导致DHT和onion 声明包绑在一起， 可能将会引进一个瑕疵在系统里作为非同盟可以配合一些人DHT和一个长期的密钥一起。/

