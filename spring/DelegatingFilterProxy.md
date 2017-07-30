��ȫ��������
Spring Security��web�ܹ�����ȫ���ڱ�׼��servlet�������ġ� ��û�����ڲ�ʹ��servlet���κ���������servlet�Ŀ�ܣ�����spring mvc���� ������û�����κ��ض���web����ǿ�й����� ��ֻ�ܴ���HttpServletRequest ��HttpServletResponse������������ʱ�����������web����ͻ��ˣ�HttpInvoker����һ��AJAXӦ�á�

Spring Securityά����һ������������ÿ��������ӵ���ض��Ĺ��ܣ���������Ҫ����Ҳ���Ӧ��Ӻ�ɾ���� �������Ĵ����Ƿǳ���Ҫ�ģ�����֮�䶼��������ϵ�� ������Ѿ�ʹ���������ռ����ã����������Զ��������ã� �㲻��Ҫ�����κ�Spring Bean��������ʱ������Ҫ��ȫ����Spring���������� ��Ϊ��ʹ���������ռ�û���ṩ�����ԣ���������Ҫʹ�����Լ��Զ�����ࡣ

1. DelegatingFilterProxy
��ʹ��servlet������ʱ�������Ҫ�����web.xml���������ǣ� ���ǿ��ܱ�servlet�������ԡ���Spring Security����������Ҳ�Ƕ�����xml�е�spring bean�� ��˿��Ի��Spring������ע����ƺ��������ڽӿڡ� spring��DelegatingFilterProxy�ṩ���� web.xml��application context֮�����ϵ��

��ʹ��DelegatingFilterProxy����ῴ���� web.xml�ļ��е��������ݣ�

<filter> <filter-name>myFilter</filter-name> <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class> </filter> <filter-mapping> <filter-name>myFilter</filter-name> <url-pattern>/*</url-pattern> </filter-mapping>
ע�������������ʵ��һ��DelegatingFilterProxy�������������û��ʵ�ֹ��������κ��߼��� DelegatingFilterProxy���������Ǵ���Filter�ķ�������application context����bean�� ����bean���Ի��spring web application context����������֧�֣�ʹ���ý�Ϊ��㡣 bean����ʵ��javax.servlet.Filter�ӿڣ��������filter-name�ﶨ���������һ���ġ��鿴DelegatingFilterProxy��javadoc��ø�����Ϣ��

2. FilterChainProxy
����Ӧ������ˣ����������ÿ��Spring Security������bean������application context����Ҫ�ġ� ��һ��DelegatingFilterProxy�����ӵ�web.xml�� ȷ�����ǵĴ�������ȷ�ġ� ����һ�ַ����ķ�ʽ������web.xml�Ե�ʮ�����ң��������������̫��������Ļ��� ����������һ����������ڣ���web.xml�У�Ȼ����application context�д���ʵ�壬 �������ǵ�web��ȫbean�� �����FilterChainProxy���������顣��ʹ��DelegatingFilterProxy ���������������������������Ƕ�Ӧ��class��org.springframework.security.web.FilterChainProxy�� ������������application context�������ġ�������һ�����ӣ�

<bean id="filterChainProxy" class="org.springframework.security.web.FilterChainProxy"> <sec:filter-chain-map path-type="ant"> <sec:filter-chain pattern="/webServices/**" filters=" securityContextPersistenceFilterWithASCFalse, basicAuthenticationFilter, exceptionTranslationFilter, filterSecurityInterceptor" /> <sec:filter-chain pattern="/**" filters=" securityContextPersistenceFilterWithASCFalse, formLoginFilter, exceptionTranslationFilter, filterSecurityInterceptor" /> </sec:filter-chain-map> </bean>
�����ע�⵽FilterSecurityInterceptor�����Ĳ�ͬ��ʽ�� �����ռ�Ԫ��filter-chain-map���������ð�ȫ���������� ��ӳ��һ���ض���URLģʽ�������������У���bean�����������filtersԪ�ء� ��ͬʱ֧��������ʽ��ant·��������ֻʹ�õ�һ�����ֵ�ƥ��URI�� �����н׶�FilterChainProxy�ᶨλ��ǰweb����ƥ��ĵ�һ��URIģʽ����filters����ָ���Ĺ�����bean�б���ʼ�������� �������ᰴ�ն����˳������ִ�У���������ԶԴ����ض�URL�Ĺ�������������ȫ�Ŀ��ơ�

�����ע�⵽�ˣ������ڹ�������������������SecurityContextPersistenceFilter��ASC��allowSessionCreation�ļ�д����SecurityContextPersistenceFilter��һ�����ԣ��� ��Ϊweb����������������������jsessionid��Ϊÿ���û���������һ��HttpSession��ȫ��һ���˷ѡ� �������Ҫ����һ���ߵȼ���߿���չ�Ե�ϵͳ�������Ƽ���ʹ����������÷����� ����Сһ�������Ŀ��ʹ��һ��HttpSessionContextIntegrationFilter��������allowSessionCreationĬ��Ϊtrue�����㹻�ˡ�

���й��������ڵ������ϣ������Щ������FilterChainProxy�Լ����ã�FilterChainProxy��ʼ�ո�����һ���Filter����init(FilterConfig)��destroy()������ ��ʱ��FilterChainProxy�ᱣ֤��ʼ�������ٲ���ֻ����Filter�ϵ���һ�Σ� ���������ڹ��������б������˶��ٴΣ�������������еľ��񣬱�����Щ�����Ƿ񱻵��� ��targetFilterLifecycle��ʼ������DelegatingFilterProxy�� Ĭ������£����������false��servlet�����������ڵ��ò��ᴫ���� DelegatingFilterProxy��

�������˽����ʹ�������������ù���web��ȫ�� ����ʹ��һ��DelegatingFilterProxy�����������ǡ�springSecurityFilterChain���� ��Ӧ�����ڿ��Կ���FilterChainProxy�����֣������������ռ䴴���ġ�

2.1. �ƹ���������
ͨ�������ռ䣬�����ʹ��filters = "none"�����ṩһ��������bean�б� ��ᳯ������ģʽ��ʹ�ð�ȫ�����������塣ע���κ�ƥ�����ģʽ��·���������κ���Ȩ��У��ķ��� �����ã������ǿ������ɷ��ʵġ�

3. ������˳��
������web.xml��Ĺ�������˳���Ƿǳ���Ҫ�ġ� ������ʵ��ʹ�õ����ĸ���������<filter-mapping>��˳��Ӧ��������������

ChannelProcessingFilter����Ϊ��������Ҫ�ض�������Э�顣

ConcurrentSessionFilter����Ϊ����ʹ��SecurityContextHolder���ܣ�������Ҫ���� SessionRegistry ���������з�ӳ���ڽ��е�����

SecurityContextPersistenceFilter������ SecurityContext������web����Ŀ�ʼ�׶�ͨ�� SecurityContextHolder������Ȼ�� SecurityContext���κ��޸Ķ�����web���������ʱ��Ϊ��һ��web������׼�������Ƶ� HttpSession�С�

��ִ֤�л��� - UsernamePasswordAuthenticationFilter, CasAuthenticationFilter, BasicAuthenticationFilter �ȵ� - ���� SecurityContextHolder ���Ա��޸ģ�������һ���Ϸ��� Authentication �����־��

SecurityContextHolderAwareRequestFilter���������ʹ��������һ��Spring Security����HttpServletRequestWrapper��װ�����servlet�����

RememberMeAuthenticationFilter���������֮ǰ����ִ֤�л���û�и��� SecurityContextHolder����������ṩ��һ������ʹ�õ�remember-me�����cookie��һ����Ӧ���ѱ���� Authentication����ᱻ����������

AnonymousAuthenticationFilter���������֮ǰ����ִ֤�л���û�и��� SecurityContextHolder���ᴴ��һ������ Authentication����

ExceptionTranslationFilter��������׽ Spring Security�쳣�����������ܷ���һ��HTTP������Ӧ������ִ��һ����Ӧ�� AuthenticationEntryPoint��

FilterSecurityInterceptor������web URI��

4. ʹ������������ ���� ���ڿ��
�������ʹ��SiteMesh��ȷ��Spring Security��������SiteMesh������֮ǰ���á� ����Ա�֤SecurityContextHolderΪÿ��SiteMesh��Ⱦ����ʱ������



5. ������������
����һ��
web.xml����һ��
    <filter>
        <filter-name>DelegatingFilterProxy</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
        <init-param>
            <param-name>targetBeanName</param-name>
            <param-value>myFilter</param-value>         //�Լ�������������
        </init-param>
        <init-param>
            <param-name>targetFilterLifecycle</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>

    <filter-mapping>
        <filter-name>DelegatingFilterProxy</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
��������
web.xml����һ��
    <filter>
        <filter-name>myFilter</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
        <init-param>
            <param-name>targetFilterLifecycle</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>

    <filter-mapping>
        <filter-name>myFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

����һ���߶���ͬ�ĵط�������web.xml�е�д����ͬ����û��̫�����������web.xml֮��Ҫ����applicationContext.xml�е�bean��
applicationContext.xml����:
<bean id="myFilter" class="com.bjtu.filter"> //ָ�������filter��
    <property name="service">                    //��Ҫע��ľ������
        <ref bean="service"/>
    </property>
</bean>
<bean id="service" parent="baseTransactionProxy">//�����service��װ�����ж����ݿ�Ĳ���
        <property name="target">
            <bean class="com.maimaiche.service.MaiMaiCheServiceImpl">
             ......
             </bean>
       </property>
</bean>

--------------------------------------------------
1��web.xml
Java����  �ղش���

    <!-- ����filter,ί�и�spring -->  
        <filter>  
            <filter-name>appFilters</filter-name>  
            <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>  
            <init-param>  
                <param-name>targetFilterLifecycle</param-name>  
                <param-value>true</param-value>  
            </init-param>  
        </filter>  
        <filter-mapping>  
            <filter-name>appFilters</filter-name>  
            <url-pattern>/*</url-pattern>  
        </filter-mapping>  



2��applicationContext-filter.xml
Java����  �ղش���

    <bean id="appFilters" class="org.springframework.security.util.FilterChainProxy">  
            <security:filter-chain-map path-type="ant">  
                <security:filter-chain filters="characterEncodingFilter,commonParamsFilter"  
                    pattern="/**" />  
            </security:filter-chain-map>  
        </bean>  
      
        <bean id="characterEncodingFilter" class="org.springframework.web.filter.CharacterEncodingFilter">  
            <property name="encoding" value="UTF-8" />  
            <property name="forceEncoding" value="true" />  
        </bean>  
        <bean id="commonParamsFilter" class="com.renren.wap.fuxi.filter.CommonParamsFilter" /> 