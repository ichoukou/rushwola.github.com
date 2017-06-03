Velocity�м���vm�ļ������ַ�ʽ

velocitypropertiespath
Velocity�м���vm�ļ������ַ�ʽ��
 
��ʽһ������classpathĿ¼�µ�vm�ļ�
Properties p = new Properties();
p.put("file.resource.loader.class",
"org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader");
Velocity.init(p);
...
Velocity.getTemplate(templateFile);
 
 
��ʽ�������ݾ���·�����أ�vm�ļ�����Ӳ��ĳ�����У��磺d://tree.vm
Properties p = new Properties();
p.setProperty(VelocityEngine.FILE_RESOURCE_LOADER_PATH, "d://");
Velocity.init(p);
...
Velocity.getTemplate("tree.vm");
 
 
��ʽ����ʹ���ı��ļ����磺velocity.properties���������£�
#encoding
input.encoding=UTF-8
output.encoding=UTF-8
contentType=text/html;charset=UTF-8
��Ҫָ��loader.

 
���������·�ʽ���м���
Properties p = new Properties();
p.load(this.getClass().getResourceAsStream("/velocity.properties"));
Velocity.init(p);
...
Velocity.getTemplate(templateFile);

package com.study.volicity;

import java.io.IOException;
import java.io.StringWriter;
import java.util.Properties;

import org.apache.velocity.app.Velocity;
import org.apache.velocity.Template;
import org.apache.velocity.VelocityContext;

public class Test {
	

	public static void main(String args[]) throws IOException {

		Properties pros = new Properties();
		pros.load(Test.class.getClassLoader().getResourceAsStream("velocity.properties"));
		Velocity.init(pros);
		VelocityContext context = new VelocityContext();

		context.put("name", "Velocity");
		context.put("project", "Jakarta");

		/* lets render a template �����Ŀ·�� */
		Template template = Velocity.getTemplate("/view/header.vm");

		StringWriter writer = new StringWriter();

		/* lets make our own string to render */
		template.merge(context, writer);
		System.out.println(writer);
	}

}