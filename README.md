# csharp2cpp
c#的结构体与c++结构体交互

# 思路

### 需求：c++导出结构体，给c#使用。关联的内容有：
	
导出函数
    [DllImport(dllPath)]
    public static extern IntPtr getSCityCsvData(int cityid);

导出的结构体：
    [StructLayout(LayoutKind.Sequential, Pack = 4/*, CharSet = CharSet.Ansi*/)]
    public class SCityCsvData
    {
        public int guid;
        public int CityType;
        public int Level;
        public int AttackedNum;
        public int AttackTeamNum;
        public int DefenceTeamNum;
        public int RefreshState;
        public int RewardId;
        public int ExtraRewardId;
        public int InquiryId;
        public int FightFieldId;
        public int Show;   // 该城池是否有效
        public int PlayerLevel;	// 玩家限制等级
    };
	
    public class CityCsvData
    {
		public SCityCsvData pSCityCsvData;
        public string CityName;
        public string CityMesh;
        public string Desc;
        public string SkillDesc;
        public string MeshParent;
        public string Icon;
        public string shader_offset;
        public string ZhouName;
		public List<int> Others;
    }

根据这些内容，自动生成c++对应的代码，要一比一生成：
	结构体。
	导出函数。
	
同时，生成c#对应的工具函数，比如：
	CityCsvData getCityCsvData(int cityid)
	{
		var CityCsvData = new CityCsvData();
		
		CityCsvData.pSCityCsvData = getSCityCsvData(cityid);
		
		IntPtr CityName = 0;
		
		getSCityCsvData_1(cityid, ref CityName);
		CityCsvData.CityName = Marshal.PtrToStringAnsi(CityName);
	}
	
    [DllImport(dllPath)]
    public static extern bool getSCityCsvData_1(int cityid, ref IntPtr CityName);
	
	
	
数据类型支持：
	不允许出现嵌套
	仅支持基础类型：int,string,float,Int64
	以及数组：List<T>
	
		


	