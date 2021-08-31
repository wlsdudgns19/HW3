# HW3

1. 스프링부트로 개발 환경 설정하기
  - 앞선 1차 과제와 비교했을때 스프링에 비해 스프링부트 개발 환경 설정이 매우 간단하다는 것을 알 수 있습니다.
  - Pom.xml에 부트 버전은 2.5.4로 해주었습니다. 이후 제 mariaDB비밀번호에 맞춰 변경해 주었습니다.
    * ![image](https://user-images.githubusercontent.com/71567319/130899007-1972c612-9e27-4df5-bb5f-9dce7102a053.png) ![image](https://user-images.githubusercontent.com/71567319/130899011-74884927-cdd3-4cca-a910-58c947ca55c2.png)

------------

2. 통계 API(SW활용현황)을 위한 DB, TABLE생성
  - 기본 requestInfo 정보 & 데이터 추가
    * ![image](https://user-images.githubusercontent.com/71567319/130961467-8884ee51-6d93-43a1-932f-c4bd4c509ec9.png) ![image](https://user-images.githubusercontent.com/71567319/130961480-402960a5-1fc0-47c5-bfc2-847c3ce93b02.png)
  - user 데이터 추가
    * ![image](https://user-images.githubusercontent.com/71567319/130961639-fadb8384-4993-4185-a548-64b4f5e668ad.png)

------------

3. [20년도 로그인 수 API] 스프링 부트, mybatis, mariadb연동
  - MybatisConfig.java
    ```
     
	@Configuration
	@MapperScan(basePackages = "com.devfun.settingweb_boot.dao")
	public class MybatisConfig {
   
		@Bean
    		public SqlSessionFactory sqlSessionFactory (DataSource dataSource) throws Exception {
        		SqlSessionFactoryBean sqlSessionFactory = new SqlSessionFactoryBean();
        
        		sqlSessionFactory.setDataSource(dataSource);
        		sqlSessionFactory.setTypeAliasesPackage("com.devfun.settingweb_boot.dto");
        
        		return sqlSessionFactory.getObject();
    		}
    
    		@Bean
    		public SqlSessionTemplate sqlSession (SqlSessionFactory sqlSessionFactory) {
        
		        return new SqlSessionTemplate(sqlSessionFactory);
    		}
	}

    ```

  - statisticMapper.xml
    ```
    <select id="selectYearLogin" parameterType="string" resultType="hashMap">
        select count(*) as totCnt
        from statistc.requestinfo ri
        where left(ri.createDate, 2) = #{year};
    </select>
    <select id="selectYearMonthLogin" parameterType="string" resultType="hashMap">
        select count(*) as totCnt
        from statistc.requestinfo ri
        where left(ri.createDate, 2) = #{year} and mid(ri.createDate, 3, 2) = #{month};
    </select>
    <select id="selectYearMonthDateLogin" parameterType="string" resultType="hashMap">
        select count(*) as totCnt
        from statistc.requestinfo ri
        where left(ri.createDate, 2) = #{year} and mid(ri.createDate, 3, 2) = #{month} and mid(ri.createDate, 5, 2) = #{date};
    </select>
    <select id="selectAvgLogin" parameterType="string" resultType="hashMap">
        select count(*) / (select count(distinct(substring(ri.createDate, 1, 6))) from statistc.requestInfo ri) as average
		from statistc.requestInfo;
    </select>
    
     <select id="selectOrganLogin" parameterType="string" resultType="hashMap">
        select count(*) as totCnt 
		from statistc.requestInfo ri, statistc.user
		where ri.userID = user.userID and left(ri.createDate, 2) = #{year} and mid(ri.createDate, 3, 2) = #{month}  and HR_ORGAN = #{organ};
    </select>
    ```

  - StatisticServiceImpl.java
    ```
    @Autowired
    private StatisticMapper uMapper;
    
    @Override
    public HashMap<String, Object> yearloginNum (String year) {
        // TODO Auto-generated method stub
        HashMap<String, Object> retVal = new HashMap<String,Object>();
        
        try {
            retVal = uMapper.selectYearLogin(year);
            retVal.put("year", year);
            retVal.put("is_success", true);
            
        }catch(Exception e) {
            retVal.put("totCnt", -999);
            retVal.put("year", year);
            retVal.put("is_success", false);
        }
        
        return retVal;
    }
    
    @Override
	public HashMap<String, Object> yearmonthloginNum(String year, String month) {
		HashMap<String, Object> retVal = new HashMap<String,Object>();
        
		try {
            retVal = uMapper.selectYearMonthLogin(year, month);
            retVal.put("year", year);
            retVal.put("month", month);
            retVal.put("is_success", true);
            
        }catch(Exception e) {
            retVal.put("totCnt", -999);
            retVal.put("year", year);
            retVal.put("month", month);
            retVal.put("is_success", false);
        }
        
        return retVal;
	}
    
    @Override
	public HashMap<String, Object> yearmonthdateloginNum(String year, String month, String date) {
		HashMap<String, Object> retVal = new HashMap<String,Object>();
        
		try {
            retVal = uMapper.selectYearMonthDateLogin(year, month, date);
            retVal.put("year", year);
            retVal.put("month", month);
            retVal.put("date", date);
            retVal.put("is_success", true);
            
        }catch(Exception e) {
            retVal.put("totCnt", -999);
            retVal.put("year", year);
            retVal.put("month", month);
            retVal.put("date", date);
            retVal.put("is_success", false);
        }
        
        return retVal;
	}
    
    @Override
	public HashMap<String, Object> avgloginNum() {
		HashMap<String, Object> retVal = new HashMap<String,Object>();
        
        try {
            retVal = uMapper.selectAvgLogin();
            retVal.put("is_success", true);
            
        }catch(Exception e) {
            retVal.put("totCnt", -999);
            retVal.put("is_success", false);
        }
        
        return retVal;
	}
    
    @Override
	public HashMap<String, Object> organloginNum(String year, String month, String organ) {
		HashMap<String, Object> retVal = new HashMap<String,Object>();
        
		try {
            retVal = uMapper.selectOrganLogin(year, month, organ);
            retVal.put("year", year);
            retVal.put("month", month);
            retVal.put("organ", organ);
            retVal.put("is_success", true);
            
        }catch(Exception e) {
            retVal.put("totCnt", -999);
            retVal.put("year", year);
            retVal.put("month", month);
            retVal.put("organ", organ);
            retVal.put("is_success", false);
        }
        
        return retVal;
	}
    ```
    
  - settingTest.java
    ```
    @Controller
    public class settingTest {
    
 
    @Autowired
    private StatisticService service;
    
    @ResponseBody 
    @RequestMapping("/login/{year}")
    public Map<String, Object> loginsqltest(@PathVariable("year") String year) throws Exception{ 
        
        return service.yearloginNum(year);
    }
    
    @ResponseBody 
    @RequestMapping("/login/{year}/{month}")
    public Map<String, Object> loginsqltest(@PathVariable("year") String year, @PathVariable("month")String month) throws Exception{ 
        
        return service.yearmonthloginNum(year, month);
    }
    
    @ResponseBody 
    @RequestMapping("/login/{year}/{month}/{imsi}")
    public Map<String, Object> loginsqltest(@PathVariable("year") String year, @PathVariable("month")String month, @PathVariable("imsi")String imsi) throws Exception{
        
    	if(imsi.length()==1) //부서입력
    		return service.organloginNum(year, month, imsi);
    	else
    		return service.yearmonthdateloginNum(year, month, imsi);
        
    }
        
    @ResponseBody 
    @RequestMapping("/average")
    public Map<String, Object> averagesqltest() throws Exception{ 
        
        return service.avgloginNum();
    }
    ```
    
------------

4. SW활용 현황 통계 API구축을 위한 SQL작성
  - 연도별 접속자 수
    * ![image](https://user-images.githubusercontent.com/71567319/130962989-5a892f26-0de6-4a93-a4b8-3c460e4ab356.png)

  - 월별 접속자 수
    * ![image](https://user-images.githubusercontent.com/71567319/130962999-9e17b285-e248-4eb8-b9cb-70e1bde4efd8.png)

  - 일자별 접속자 수
    * ![image](https://user-images.githubusercontent.com/71567319/130963013-640d9290-9b67-495e-8a1c-4026e568cf78.png)

  - 평균 하루 로그인 수
    * ![image](https://user-images.githubusercontent.com/71567319/130963037-2910844b-7740-479b-9117-475a9952ce97.png)

  - 부서별 월별 로그인 수
    * ![image](https://user-images.githubusercontent.com/71567319/130963065-6245473c-4510-44ab-aad6-8a632f9479f4.png)

  - 휴일을 제외한 로그인 수 (API구축시 추가 구현 필요, 별도의 db생성 또는 공공API활용)
    * 4차 과제에서 진행

------------

5. 질문
  - 부서별 월별 로그인 수를 구하는 과정에서 '연도/월/부서' 이렇게 받고 싶은데 이전에 일자별 접속자 수를 구하는 과정에서 '연도/월/일'과 겹처 잘 되지 않는 것 같습니다.
  - settingTest.java에서 어쩔 수 없이 size가 1이면 (A or B or C or D ...) 부서로 인식하게 해주고 아니면 date로 인식되도록 해주었습니다.
  - 어떻게 하면 따로 처리가 가능한지 궁금합니다.
