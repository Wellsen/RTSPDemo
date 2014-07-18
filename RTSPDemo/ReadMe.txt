// "RZ" is short for Ricci.Z ;-)
��RTSPClient/0.0.1 (HP Streaming Media v2014.05.24)
		->һ�����̵߳ĳ����汾
��RTSPClient/0.1.1 (HP Streaming Media v2014.07.01)
		->������̺߳���־ϵͳ
//////////////////////////////////////////////////////////////////////////
RZNetConn		[Connection.h]
����һ�������࣬���ܹ���ʵ�����������ṩ��һЩ��������ӹ��еĹ����ӿڣ����Ҷ�����
���е����඼���õ���һЩ���Ժͷ������������಻���ٵ������壬ֱ�Ӽ̳оͿ�����
ͨ������һ������������˵��socket/ip/port���Ǳ���ģ���Щ�ǹ���һ��ͨ���ŵ���
������ʩ��ע������һ�������������ʹ�õ�Э�����ͱ����ڶ˿ڶ����У�������Ϊ�ж˿�
��һ���������ӣ��������������ôһ�����ж˿�
WSADATA wsa;
void InitWinSockDLL();
		->��windows����һ����ʼ��windows socket dll�ĺ���
			��ʹ���κ��׽���ǰ����Ҫ�ȵ������������������Ҫʹ���׽���
void InitNetConn();
		->��ʼ���������ӣ���Ҫ���������ڽ������ݵĻ�����������ն����ϣ���ִ��һЩƽ̨
			��صĳ�ʼ��

int iSockFile;
		->iSockFile = -1 ��Ч��������Ч
void CreateSocket();
		->�����׽��֣����ݶ˿��д�ŵ�Э������������TCP����UDP�׽���
inline bool SocketValid() const;
		->���Ե�ǰ�׽����Ƿ���Ч���ڽ�������֮ǰ����Ӧ�����׽����Ƿ���Ч

RZNetIPAddr	nIPAddr;		//n means net
RZNetPort			nPort;
		->���IP��ַ�Ͷ˿ڹ��ɵĶ�Ԫ���ʶһ��ͨ�ŶԶ˵�ָ������
			������Ҫ��������ж���һ���������ض˿ڵ����ݳ�Ա��������Ϊ��Щ����
			�ı��ض˿���Ҫ�ֶ����ã�����Щ���ӵı��ض˿�����ϵͳ�ṩ
virtual void BuildConnection() = 0;
		->�������ӣ���ȻUDPЭ����������ӵķ�ʽ�ͶԶ˽���ͨ�ţ�������������Ȼ
			�ṩһ���������ӵķ�����ֻ�����ھ���ʵ��ʱ��������connect������������
			ʹ������ģ�͸���ͳһ�淶
virtual void SendDataToPeer(const std::string&) = 0;
virtual std::string RecvDataFromPeer() = 0;
		->��Զ˷��������Լ��ӶԶ˽������ݣ�����Ҫ�Ծ�������Ӷ���
inline std::string GetIPAddr() const;
		->��ȡ��ǰ���ӵĶԶ˵�IP��ַ
inline unsigned short GetPort() const;
		->��ȡ��ǰ���ӵĶԶ˵Ķ˿ں�
inline void SetPeerIPAndPort(const RZNetIPAddr&, const RZNetPort&);
		->����ͨ�ŶԶ˵�IP��ַ�Ͷ˿ڣ�����������о�������÷�ʽ

char* rBuffer;			//r means receive
unsigned long ulBufSize;
		->���Ӷ����͵��������û�ָ������˲���Ҫ���ͻ����������ӱ���ֻ����
			�������ݣ�������ʱ��Ҫ���յ����ݴ�С��δ֪�ģ����Ծ���Ҫ����һ���㹻
			��Ļ�����������������õ�Ĭ�ϻ�������С��4096�ֽ�
int GetSysRecvBufSize() const;
		->��ȡϵͳЭ��ջʹ�õĻ�������С���������ֵ�����ʵ��ĵ���Ӧ���ڲ���
			��������С
void SetSysRecvBufSize(int);
		->����ϵͳЭ��ջʹ�õĻ�������С
void SetLocalBufSize(unsigned long);
		->���õ�ǰ���ӱ������ڽ������ݵĻ������Ĵ�С����ʱû���κζ���ʹ�ã�

fd_set readSet;
int maxFd;	
		->��������ӻ��õ��ȴ�ͨ�ŶԶ���Ӧ�ĺ������п�����ͬ���ģ�Ҳ�������첽��
			�ȴ�����������select�����������Ҫһ��fd_set���͵Ķ�����
inline void ClearReadSet();
		->���������
inline void AppSockInReadSet();
		->��������ӵ��׽��ּ�������ϣ�ͬʱ����maxFd
int WaitForPeerResponse(const WAIT_MODE&, long _milliSec = 0) const;
		->�ȴ�ͨ�ŶԶ���Ӧ������ʹ��ͬ�����첽����ģʽ����Ϊ�첽���ɼ������õȴ�ʱ��
			_milliSec�ĵ�λΪ���룬����ֵΪ�������ļ�����������
inline bool HaveDataToRead() const;
		->���첽ģʽ���Ե�ǰ���ӵ��׽����Ƿ�ɶ�

//////////////////////////////////////////////////////////////////////////
RZTcpConn		[Connection.h]
�����û��˽�����ݳ�Ա�������е����ݶ��Ǵ�RZNetConn��̳е�����
virtual void BuildConnection();
		->���IP��ַ�Ͷ˿���Ч����ô�����׽��ֲ����ӵ�ָ����ַ
virtual void SendDataToPeer(const std::string&);
		->�����ݷ��͵�ͨ�ŶԶˣ����ݴ����string�ַ�����
virtual std::string RecvDataFromPeer();
		->�������ݲ����������ڲ��������У�����������̳���RZNetConn��

//////////////////////////////////////////////////////////////////////////
RZUdpConn		[Connection.h]
ʵ���϶���UDP������û��Client��Server������ģ���Ϊ�����2��ͨ�Ŷ���֮���ǶԵȵ�
��Ҳ����˵���ڱ��ػ���һ��ָ���Ķ˿ڣ������ĸ�ip��ַ��ֻҪ�������ݷ�������˿�
��ôrecvfrom������ö��Ḻ������ݽ��յ�ָ����������
��Ҫ��������ض˿���Ϊ��˽�����ݳ�Ա������������ΪRTSPClient�ںͶԶ�ͨ��ʱ��Ҫ�õ�
�������
virtual void BuildConnection();
virtual void SendDataToPeer(const std::string&);
virtual std::string RecvDataFromPeer();
		->ͬ��
inline RZNetPort GetLocalListenPort() const;
inline void SetLocalListenPort(const RZNetPort&);
		->��ȡ�����ñ��ؽ������ݵĶ˿�

//////////////////////////////////////////////////////////////////////////
RZLogger		[Logger.h]
��־��Ӧ����2�����֣���д����̨����д�ļ��������ı��ĵ��ͽṹ�������ݿ��ļ���
��д����̨���ⲿ����Ҫ���ڵ��ԣ��������ƽ̨���APIʱ���ܻ᷵�س���������Ϣ��ʾ
����Դ��չʾ�����룬��Ҫ���ڿ��ٶ�λbug�����bug��һ�����ʱֻҪд����̨�Ϳ�����
��д�ļ������ڿ����������еĳ����п��ܣ�����һ��Server����Ҫһ��7*24Сʱ�����У����
ʱ����Ҫ������һЩ��¼�������û���������Ϣ����Ӧ����ȵȣ�����Щ��¼�������۲��������
�Ƿ��������У�һ����Щ��¼���Ǽ򵥵�д�ļ������������Ҫ����ϸ�Ŀ��ƣ�����ܻ�������
�ı����ṹ���ش洢��Щ��¼������������п��ܻ�д���ݿ��ļ���Ŀǰֻ����д����̨���֣�
static void ERR(const char* fmt, ...);
		->��ʾ������ӡ������ļ��������Լ��кţ�����ӡһЩUser-Defined Information
			��Ϊ��windows VS�ϵ��Գ�����ܻ�ʹ��Ļһ�����������Լ��밴������˳�����

//////////////////////////////////////////////////////////////////////////
RZNetPort			[NetBase.h]
����಻����Ĭ�Ϲ��캯�����ڶ��������Ķ����ͬʱ����ָ���˿ڶ�Ӧ��Э������
���ȷʵ�޷�ָ��Э�����ͣ���ô��ʹ��ENUM_UNKö��ֵ
inline bool PortValid() const;
		->���Ե�ǰ�˿�ֵ�Ƿ����
inline NET_PRTTYPE GetPrtType() const;
		->��ȡ�˿ڶ�Ӧ��Э������
inline void SetPrtType(NET_PRTTYPE);
		->���ö˿ڶ�Ӧ��Э������
inline unsigned short GetPortValue() const;
		->��ȡ��ǰ�˿�ֵ
void SetPortValue(unsigned short);	
		->���ö˿�ֵ
void RandSelectValidPort(NET_PORTTYPE);
		->���ѡ��ָ����Χ�ڵ�һ����Ч�˿ڣ�ѡ��Ķ˿�����ż����������Ϊ
			RTPЭ��ֻ��ż���˿ڣ�
bool PortOccupied(unsigned long);
		->���Բ���ָ���Ķ˿�ֵ�Ƿ��ѱ�����������ռ��
NET_PORTTYPE GetPortType() const;
		->ȡ�ö˿ڵ����ͣ��Т�֪���˿ڣ���ע��˿ڣ��۶�̬�˿��������ͣ�
			Ŀǰ�������û��ʲô�ã���д�������
inline void UpdateSystemOccupiedPort();
std::vector<unsigned long> GetLocalUsedPort(NET_PRTTYPE);
std::vector<unsigned long> GetLocalUsedTcpPort();
std::vector<unsigned long> GetLocalUsedUdpPort();
		->ȡ��ϵͳ������ʹ�õĶ˿ڣ�ƽ̨��أ�

//////////////////////////////////////////////////////////////////////////
RZNetIPAddr		[NetBase.h]
������ṩһ��Ĭ�Ϲ��캯����һ�����ַ��������Ĺ��캯��������ʹ��һ�����
ʮ������ʽ���ַ�����ʼ�������Ķ���
inline unsigned long GetULIPAddr() const;
		->ȡ��IP��ַ�ĵ��ʮ���ƴ���ʽ
inline std::string GetSTRIPAddr() const;
		->ȡ��IP��ַ���޷���������ʽ
void SetIPAddr(const std::string&);
		->����IP��ַ
inline bool IPAddrValid() const;
		->��ǰIP��ַ�Ƿ���ã����õ����������ʾ���IP��ַ�Ƿ�Ϸ�
inline void InitIPAddr(const std::string&);
		->��ʼ��IP��ַ����Ҫ����2��˽�����ݳ�Ա
bool IPAddrLegal(const std::string&) const;
		->���IP��ַ�Ƿ�Ϸ����Ϸ�����true������Ϊfalse

//////////////////////////////////////////////////////////////////////////
RZReqLine			[NetBase.h]
RZExtraHdr
RZReqPacket
��3�����ṩһ������TCPЭ����������3���඼�Ƚϼ򵥣����õĵط���Ƚ϶�
��װһ����������ܷ��㣬�����ṩ�Ľӿ�Ҳ�Ƚ��٣��������������

//////////////////////////////////////////////////////////////////////////
RZGlobalInit		[MetaObject.h]
�������õ���ģʽ��������һ������ľ�̬��Ա��������ڽ���main����֮ǰ�͵�����
�����Ĺ��캯����
RZGlobalInit()
		->��������ɶ�����ϵͳ�ĳ�ʼ������������ڳ������������������ֻ������һ��

//////////////////////////////////////////////////////////////////////////
RZStream		[MetaObject.h]
������ṩ��һ�����е������п����õ��Ĵ��ָ��
static std::vector<std::string> StreamSplit(const std::string& _p1, const char* _p2);
		->���ָ��������_p2���ַ���_p1�и����_p1��������_p2�ָ����Ӵ����������ָ���

//////////////////////////////////////////////////////////////////////////
RZTypeConvert		[MetaObject.h]
������ṩ���ܻ��õ��ĸ�������֮���ת��
static int StrToInt(const std::string&, int _base);
		->����_base���ַ���ת��Ϊint��ֻ֧��2~16����֮����ַ�����ת��
static std::string IntToString(const int&);
		->������ת��Ϊ�ַ���

//////////////////////////////////////////////////////////////////////////
RZThread		[MetaObject.h]
������ƽ̨��ص��̺߳�������Ӷ�ʵ��һ��������߳��࣬��Ҫ������һ���߳���������
����������ֱ�Ӽ̳�����࣬����ֻҪʵ���̵߳��������Ϳ��Ե���StartThread�Կ���һ��
�߳�
void StartThread(int nCount = 1);
		->��ʼһ���̣߳��ο���ʾ�����̵߳ĸ�����Ĭ��Ϊ1
virtual void ThreadProc() = 0;
		->�߳��������������������Ӧ���Լ�ʵ���������
static unsigned long InitThreadProc(void* lpdwThreadParam);
		->windowsƽ̨�ϴ����߳���Ҫ�õ��Ļص��������ڸ��߳��е���srand(time(NULL))ʹ�õ�ǰʱ��������
			��û�õģ���Ϊrand()���ڸ��߳��Ƕ����� ������˵ÿ���߳����߳��л�ʱ�����浱ǰ�����״̬�����
			�ڿ���һ�����߳�ʱ����ȵ���srand����

//////////////////////////////////////////////////////////////////////////
RZAgent : public RZThread		[MetaObject.h]
RZAgent�Ǿ���Agent��һ��������
virtual void ConnetToPeer(const RZNetIPAddr&, const RZNetPort&);
		->����Agent���������ӵ��Զ˵Ĳ�������ĳ��Agent�Ƚ�����ʱ������д�Լ����ⲿ�ֲ�����
			����TCPЭ���ڽ�������ʱ�Żᴴ���׽��֣����������������ֻ�Ի���TCPЭ�������׷
			�Ӷ�����
virtual void RecvPeerData(WAIT_MODE _eWaitMode = ENUM_SYN);
virtual void ParsePacket(std::string&) = 0;
		->���նԶ����ݲ������յ������ݣ����նԶ����ݵĲ�������ͬС�죬��˸ó������ṩһ��
			�Ѿ�ʵ�ֵ��麯�����������Agent���Լ��������ݵķ�ʽʱͬ��������д�������
			ParsePacketָ�������յ������ݵķ���
void InitLocalPort(const RZNetPort&);
		->��ʼ�����ض˿ڣ�ֻ��ʹ��UDPЭ��ʱ�Ż�������������������Ϊʹ��TCP����ʱ������
			�˿�����ϵͳ�Զ�ѡ���һ������˿ڣ���ʼ���������Ƚ����ض˿���ָ���˿ڽ��а󶨣�
			���ڰ󶨲���ʱ�ᴴ���׽��֣�����ڰ����֮�󣬽��׽���׷������������

//////////////////////////////////////////////////////////////////////////
RZSessDescribe		[SessDescribe.h]
�������Ҫ���ڴ�Ź���ĳ��ԭ�ļ�����Ϣ����������ļ���Ҫ��ͨ�����˽��д��䣬����ڴ���
֮ǰ��Ҫ���ȸ�֪�Զ�����ļ�����Ϣ�����ڷ�����˵���������Ҫ�Ĳ����Ǵ棬����������
˵����ȡ
��������л�������һ��SDPCacheInfo�ṹ���洢����ṹ��Ϊ�˼���ĳЩ����������ΪƵ����
���ݵļ����������õ���Щ����ʱ��ֱ��ȡ����õĻ����е����ݾͿ�����
inline std::string GetAudioID() const;
inline std::string GetVideoID() const;
		->��ȡsdp�ṹ���б�ʶ����Ƶ����ID
void SetTypeValue(const std::string&);
		->���ַ���������sdp�ṹ�壬�����������Լ�ý������ID
			������ý����ID����ڻ�����Ϣ�ṹ��SDPCacheInfo��
void SetAVStreamIndex();
void SetMediaStreamID(const MediaDescription*);
		->��������Ƶ�������Լ�ID

//////////////////////////////////////////////////////////////////////////
RZRTPAgent : public RZAgent		[RTPProtocol.h]
RZRTPAgent��ʱ��Ҫ���ڽ��նԶ˵�ʵʱ����������������������UDPЭ�鷢�͵�
��������õ��˱Ƚϸ��ӵ��Զ���Ľṹ�壬��Щ�ṹ�����ֶε���ϸ��������
struct _RTPHeader 
{
	RTPFirstByte		uFirByte;
	RTPSecByte		uSecByte;
			->����2��������ÿ���ֶΣ�����λ�򣩵Ľ��ʹ����Դ�ļ��Ķ��崦
	unsigned short		SequenceNumber;
			->���кţ����ͷ���ÿ����һ������ͽ���ֵ����1�����շ��������������
				���Ķ�ʧ�Լ��ָ��������У����кŵĳ�ֵ�������
	char		TimeStamp[4];
			->ʱ�������¼�˸ð��е����ݵĵ�һ���ֽڵĲ���ʱ�̣���һ�λỰ��ʼʱ��
				ʱ�����ʼ����һ����ʼֵ��ʱ�������ֵҪ��ʱ�䲻�ϵ�����
	unsigned long SSRC;
			->ָʾRTP��������Դ����ͬһ��RTP�Ự�в�����2����ͬ��SSRCֵ
	std::vector<std::string>		CSRC;
			->����Դ�б���0~15�ÿ��32���أ�������ʶ��һ��RTP������������°�
				�й��׵�����RTP����Դ������б���ӵ�е����ǲ�ȷ���ģ�������FirstByte��
				��CC��־λָ��
};

struct _MediaMetaData;
		->������ʵʱ��ʱ���п��ܶ�Ӧ���ļ��ǳ��󣬴Ӷ�������ڴ�����ɻָ������е�
			��������ռ���൱����ڴ���Դ������취�ǰ���UDP���ݰ��е�RTPͷ������
			������ڴ��У�����UDP�е�ʵʱ�����ִ洢��һ����ʱ�ļ��У���������ʱ�ļ�
			�н������δ��UDP���ݰ��е�ʵʱ���������Ҫ���ڴ��д���ļ��ڵ�ʵʱ����
			��Ϣ����������ṹ����Ϊ��ʱ�ļ�������������������Ԫ���ݶ����ڣ��ڻָ���
			����ʱֻҪ�ȶ�����ṹ��������Ϳ����ˣ��Ӷ�����ؽ�Լ���ڴ���Դ

struct _MediaPacketInfo;
{
			->���յ�������Ҫ�����´������RTP�е�P�򡪡���λָʾ����β���Ƿ�����䣬
				���û��䣬��ô����������RTP�����������䣬������������ֽڱ�ʾ����
				���ȣ�Ӧ��Ҫ���������ȥ�������ֽ�
	int iFirstSeq;
	int iPackNum;
	bool bRecvAll;
			->���յ����һ��RTCP��ʱ�����п��ܲ�δȫ�������꣬��Ϊ�������͵�
	unsigned long ulSSRC;
			->���ͬ��Դ��RZRTSPAgent���ã���ÿ�ν���RTP��ʱ��Ҫ������ͬ��Դ��ʶ
				�������ֵ�ȶԣ�����ͬ�ټ����������򶪰�
	FILE* pTmpFile;
	FILE* pOrdFile;
			->��ʱ�ļ������Կɶ���д�ķ�ʽ�򿪣���Ϊ�䲻���漰��������ʵʱ����������
				�������������֮�󣬻���������е�ָ��λ�ö�ȡ���ݿ�Ӷ��ָ�������ȷ˳��
				�������ļ��ĳ�ʼ��������RZRTPAgent�Ĺ��캯���н��У�������Ϊ�������RTP
				��Ҫ��ɵ�������������Ӧ���ļ��������ļ�ָ����ͷ�Ҳ������RZRTPAgent������
	std::vector<MediaMetaData> vMetaData;
};
������RZRTPAgent���õ��Ľṹ��
std::queue<std::string> qBufferPool;
		->����һ������أ������յ�һ��udp��ʱ������������д���ļ������ǵȵ��������ʱ
			��һ��д���ļ��������ܹ����ϵͳ��I/OЧ��
void InitAgent();
		->��ʼ���������趨���صĽ��ն˿ڣ�ѡ��Ķ˿ڱ���������2����δ��ռ�õĶ˿��е�
			��һ�������ҵ�һ���˿�Ӧ����ż�������ڶ����˿�����RTCPЭ��ʹ�á���2���˿���
			��̬�˿��н���ѡ��
void InitTmpFile();
		->��ʼ����ʱ�ļ������ݽ���������������������Ӧ���ļ�������Щ�ļ�����������
			���������йر���Щ�ļ�
void ThreadProc();
void ParsePacket(std::string&);
void WriteTmpFile();
static bool Compare(const MediaMetaData&, const MediaMetaData&);
void SortPacket();
		->ThreadProc���߳�����������������߳�ʱ���ȴ�RTCP�̣߳�����ͣ�ؽ��նԶ˷���
			�����ص��������������յ�һ�����ݰ�֮��ʹ��ParsePacket�����������������������
			��Ч�غɴ洢�ڱ��ػ���أ���������������ѽ�����ȫ��������֮���ٵ���
			WriteTmpFileд����ʱ�ļ���ע���ʱд���������������ġ����������е�������֮��
			�˳���ѭ������������������������������ļ�
inline RZNetPort GetLocalPort() const;
		->��ȡ���صĽ��ն˿�
inline void SetMediaType(RTP_MEDIA_TYPE);
		->����ý������
inline void SetFirstSeq(int);
		->���õ�һ��RTP�������к�
inline void SetSSRC(unsigned long);
		->����RTP����SSRC��ͬ��Դ��ʶ����
inline bool PackRecvComplete() const;
		->����RTP���Ƿ���ȫ�������꣬�����򷵻�true�����򷵻�false

//////////////////////////////////////////////////////////////////////////
RZRTCPAgent : public RZAgent		[RTPProtocol.h]
RTCP��RTP�Ŀ��Ʋ��֣�����ҪΪRTPʵʱ���ķ��������ṩ��֤��������RTCP���в�����
ʵ�壬ֻ��ͷ��������һ��RTCP��ͨ����������ͷ����
RTCP������5�����͵�ͷ�����ֱ��ǣ�
��SR��200���������Ͷ˱��棻��RR��201���������ն˱��棻��SDES��202������Դ������
��BYE��203�������������䣻��APP��204�������ض�Ӧ��
ͨ����һ��ͷ���ĵڶ����ֽڣ�PT��ָ��ͷ��������
�����ʵ����ֻ���Ǵӷ���������������������ʵʱ�����������ƣ�����ֻ����SRͷ���ṹ��
��Ϊ���а�����RTP���ݰ��йص���Ϣ��SenderInfo�е�PacketCount/PayloadOctets�ֶΡ���
�ڶ���RTCP���е�����ͷ��ʱ�򵥺��ԣ�����⵽BYEͷ��ʱ�Ϳ���֪���ѷ��͵�RTP��������
RTCP_Packet rPacket;
		->��ʾ�ӶԶ��յ�����Ӧ��
inline void SetLocalSSRC(unsigned long);
		->���ñ���SSRC��ͨ���ڿ�ʼ����RTCP��֮ǰ���ã����յ�RTCP��֮��Ҫ���SSRC�Ƿ�
			���
inline bool GetLastPacket() const;
inline unsigned long GetTotalRTPPack() const;
		->ָ���Ƿ���ȡ�����һ��RTCP��������ȡRTP�����������ڽ��յ����һ��RTCP��ʱ��
			������к��жԶ˷��͵�RTP��������
void ThreadProc();
void ParsePacket(std::string&);
		->ThreadProc���߳�����������������߳���Ҫ���ڽ���RTCP������������Щ����Ӧ������
			��Ŀ����ȡ�����һ��RTCP�����õ��Զ˷��͵�RTP��������
			ParsePacket��Ҫ�ǽ���RTCP��

//////////////////////////////////////////////////////////////////////////
RZRTSPAgent : public RZAgent		[RTSPAgent.h]
RTSPͨ����RTP/RTCPЭ����Ͻ�������ʵʱ���䣬RTSP�����������TCP���ӣ���������
��ͨ������UDP��RTPЭ����д���
RTSP_TASK			eTask;
		->�������񣬼����Agent��Զ˶�ͨ����Ϊ�����ʲô
std::string strReqFile;
		->RZRTSPAgent����õ���ʵʱ����������ĳ��������ļ���Ӧ��
RZRTPAgent* pRTPAgent;
		->���ڽ���ʵʱ����������Ϊʵʱ�������ǵ�������Ƶ������������Ƶ����Ҳ�����ǻ�ϵ�
			����Ƶ�������Կ���ֻ��Ҫ1������Ҳ���ܻ���Ҫ2������
struct _RTSPHdrCache
{
	int iCseq;
			->��ʾ��ǰ��������кţ�ÿ����һ���������кŶ�����1
	std::string strSession;
			->Sessionͷ�״γ�����Server��SETUP�������Ӧ�У����ͷ����Server���ȷ���
				һ��������Ӧ���г���Sessionͷ����ô������ÿ��������ж���Ҫ����������ͷ
};
ResponsePacket	stResPacket;
RTSPHdrCache		stHdrCache;
RZSessDescribe	sdpPacket;
		->�ֱ��ʾ�ӶԶ˽��յ���Ӧ�����Լ���Ӧ���еĻ�����Ϣ���Ự����ʵ��
void ConnectToServer(const char*);
		->���ӵ���������֮���Բ���Ҫָ���˿ڣ�����Ϊ����ʹ����RTSPͨ�ŵ�Ĭ�϶˿�
inline void SetRequestFile(const std::string&);
		->���������ļ�
void StartTask(RTSP_TASK);
		->ִ������
std::string			GetMethodStr(RTSP_METHOD) const;
		->����ö��ֵ��ȡ��Ӧ�������ַ���
RZReqLine		GetReqLineByMethod(RTSP_METHOD, int);
		->���ݷ�����ȡ��Ӧ�������С�һ����������<method> <uri> <version>��ɣ�����uri��
			��ͨ����Դ��λ����ͨ��ֻ���õ�ָ���ĵ�ַ��ָ�����ļ���������RTSP�������ʵʱ��
			����Ƶ��������2�����ַ�����һ��ͳһ���ļ��У����RTSP��ĳЩ������Ӧ��uri����
			������Ҫָ���ļ��ڲ���ĳһ�����ݣ���������Ƶ��Ҳ��������Ƶ����������uri������
			rtsp://ipaddr:port/requestfile������rtsp://ipaddr:port/requestfile/specifydatablock
			��ʹ�õ���һ��uri�ķ���ֻ���������������Ҷ������������˵ֻ��SETUP�������õ�
std::string			GetHeaderFieldStr(RTSP_HEADER_FIELD) const;
		->����ö��ֵ��ȡ��Ӧ��ͷ����ַ���
std::vector<RZExtraHdr> GetGeneralExtraHdr() const;
void GetOPTIONSExtraHdr(std::vector<RZExtraHdr>&) const;
void GetDESCRIBEExtraHdr(std::vector<RZExtraHdr>&) const;
void GetSETUPExtraHdr(std::vector<RZExtraHdr>&, int) const;
void GetPLAYExtraHdr(std::vector<RZExtraHdr>&) const;
void GetTEARDOWNExtraHdr(std::vector<RZExtraHdr>&) const;
		->��ȡͨ�õĸ��ӱ�ͷ��ָ�������ĸ��ӱ�ͷ������ÿ�ַ�������Ҫ������ͷ��û��һ��
			��׼�Ĺ涨���������ο�VLC�ͻ���ʵ����
void OnDispatchAcceptAVStream();
		->ʵ�ֽ���ʵʱ���ľ��巽��
std::vector<RTSP_METHOD> GetTaskMethodList() const;
		->�������������Ӧ�ķ����б����������ļ��е���/��Ƶ���ο�VLC�е�ʵ�֣�
			���嵽�����ķ���˳�����£�OPTIONS->DESCRIBE->*(SETUP[Audio])->
			*(SETUP[Video])->PLAY->TEARDOWN
void SendRequest(RTSP_METHOD, int);
		->�����������������ˣ���ÿ�η���֮ǰ��Ҫ���Ƚ����к�����1
void ParsePacket(std::string&);
		->������Ӧ�������stResPacket��Ա��������ÿ�����֮ǰ�����������Ա��գ�����ζ��
			��ĳ��ʱ����ϣ��ó�Ա�ڵ�ÿ���ֶβ��Ƕ����ڵ�ǰ�������Ӧ�����ڽ�������Ӧ�кͱ�ͷ
			֮����ܻ���ʵ�壬����ʵ�岿�ֲ����κν�������Ϊ��ͬ������ܻ�ʹ�ò�ͬ���ı�����
			ʵ�岿�ֵĽ����ɾ����������ִ��
std::map<std::string, int>					GetRTPFirstSeq() const;
		->��ȡRTP�������е�һ���������кţ������п��ܷ��Ͷ����ͬ���͵�����������������Ӧ
			����RTP-Info��ͷ�л��ж���ԡ������ָ���ֵ����������ķ���ֵ��һ��map����������
			key��ʾ�������ı�ʶ������uri���ɡ�/���ָ������һ���ַ�����value��ʾ�������к�
std::map<std::string, std::string>		GetResTransFields() const;
		->��ȡ��Ӧ����transport��ͷ���йضԶ˶˿ڵ���Ϣ���ú���Ҳ����һ��map����������key
			��ֵΪ"server_port"��value��ʾserver_port��ֵ��������"57402-57403"
void OnDispatchAcceptAVStream();
		->������RTSPЭ�������֮���ͨ��Ӧ����ͬ���ģ�����֮�����ͻ��˷���һ������֮��
			�ͻ���Ӧ���ڴ˵ȴ���ֻ�еõ���Server�˵���Ӧ���ܸ�����Ӧ�����ݾ����Ƿ��������
			��һ�������������֮���ͨ�����첽�ģ����Կ�������һ��״��������������һ��
			DESCRIBE����֮�󣬲����ȴ��������Ļظ�����������SETUP����������Ȼ�Ǵ���ģ�
			��ΪSETUP�����о��嵽��ĳһ�����ݣ���Ƶ/��Ƶ������صı�ʶ�������ʶ����URI��
			��û�б��ͻ��˽��յ�
			�ڽ��յ�PLAY��������Ӧ��֮��RTSPAgent��������ֱ��RTPAgent������ȫ����ʵʱ
			����������Ӧ����ֻ��Range��RTP-Info��2����ͷ��Ҫ�ر�����Range�Ĵ���ʽ��
			ֱ����ʾ�ñ�ͷ�����ݣ���RTP-Info��ͷ�е�ĳЩֵ��Ҫ�ڽ���ʵʱ��ʱ��RTPAgentʹ��