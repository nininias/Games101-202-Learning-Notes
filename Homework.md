# Games101

------

## Homework3

锯齿很严重，差不多搞出来。回来的时候重新看一下超采样的部分

![output](D:\TyporaImage\output.png)







和别人的好像不太一样

![output](D:\TyporaImage\output-1728743760337-2.png)

别人的都是：

![img](D:\TyporaImage\e62e7cfe91f9253b815e0a57f15518ad.png)



调高了pow

![output](D:\TyporaImage\output-1728750782057-8.png)

逛了一圈不知道怎么回事漫反射就有了。懂了，衰减值没有弄！

```C++
auto atten = (light.position - point).dot(light.position - point); 
```





修改了uv坐标钳制到[0,1]之后，奶牛成功被贴上贴图了！

![output](D:\TyporaImage\output-1728753476493-10.png)





挺奇怪的看不到凹凸

![output](D:\TyporaImage\output-1728801570974-1.png)







现在有了，原本是先Vector3f两个颜色值减完再来做norm()，我改成了先做norm()再来做减法

![output](D:\TyporaImage\output-1728801992591-3.png)





然后是displacement

加上光照

位移顶点

![output](D:\TyporaImage\output-1728803796704-5.png)



所有的代码：

```C++
#include <iostream>
#include <opencv2/opencv.hpp>

#include "global.hpp"
#include "rasterizer.hpp"
#include "Triangle.hpp"
#include "Shader.hpp"
#include "Texture.hpp"
#include "OBJ_Loader.h"

Eigen::Matrix4f get_view_matrix(Eigen::Vector3f eye_pos)
{
    Eigen::Matrix4f view = Eigen::Matrix4f::Identity();

    Eigen::Matrix4f translate;
    translate << 1, 0, 0, -eye_pos[0],
        0, 1, 0, -eye_pos[1],
        0, 0, 1, -eye_pos[2],
        0, 0, 0, 1;

    view = translate * view;

    return view;
}

Eigen::Matrix4f get_model_matrix(float angle)
{
    Eigen::Matrix4f rotation;
    angle = angle * MY_PI / 180.f;
    rotation << cos(angle), 0, sin(angle), 0,
        0, 1, 0, 0,
        -sin(angle), 0, cos(angle), 0,
        0, 0, 0, 1;

    Eigen::Matrix4f scale;
    scale << 2.5, 0, 0, 0,
        0, 2.5, 0, 0,
        0, 0, 2.5, 0,
        0, 0, 0, 1;

    Eigen::Matrix4f translate;
    translate << 1, 0, 0, 0,
        0, 1, 0, 0,
        0, 0, 1, 0,
        0, 0, 0, 1;

    return translate * rotation * scale;
}

Eigen::Matrix4f get_projection_matrix(float eye_fov, float aspect_ratio, float zNear, float zFar)
{
    // TODO: Use the same projection matrix from the previous assignments
    Eigen::Matrix4f projection = Eigen::Matrix4f::Identity();

    Eigen::Matrix4f frustom = Eigen::Matrix4f ::Identity();
    float tanHalfFov = tan(eye_fov / 2.0f);
    frustom << 1.0f / (aspect_ratio * tanHalfFov), 0.0f, 0.0f, 0.0f,
        0.0f, 1.0f / tanHalfFov, 0.0f, 0.0f,
        0.0f, 0.0f, -(zFar + zNear) / (zFar - zNear), -2.0f * zFar * zNear / (zFar - zNear),
        0.0f, 0.0f, -1.0f, 0.0f;

    projection = frustom * projection;
    // TODO: Implement this function
    // Create the projection matrix for the given parameters.
    // Then return it.

    return projection;
}

Eigen::Vector3f vertex_shader(const vertex_shader_payload &payload)
{
    return payload.position;
}

Eigen::Vector3f normal_fragment_shader(const fragment_shader_payload &payload)
{
    Eigen::Vector3f return_color = (payload.normal.head<3>().normalized() + Eigen::Vector3f(1.0f, 1.0f, 1.0f)) / 2.f;
    Eigen::Vector3f result;
    result << return_color.x() * 255, return_color.y() * 255, return_color.z() * 255;
    return result;
}

static Eigen::Vector3f reflect(const Eigen::Vector3f &vec, const Eigen::Vector3f &axis)
{
    auto costheta = vec.dot(axis);
    return (2 * costheta * axis - vec).normalized();
}

struct light
{
    Eigen::Vector3f position;
    Eigen::Vector3f intensity;
};

Eigen::Vector3f texture_fragment_shader(const fragment_shader_payload &payload)
{
    Eigen::Vector3f return_color = {0, 0, 0};
    if (payload.texture)
    {
        Eigen::Vector2f uv = payload.tex_coords;
        return_color = payload.texture->getColor(uv.x(), uv.y());
        // TODO: Get the texture value at the texture coordinates of the current fragment
    }
    Eigen::Vector3f texture_color;
    texture_color << return_color.x(), return_color.y(), return_color.z();

    Eigen::Vector3f ka = Eigen::Vector3f(0.005, 0.005, 0.005);
    Eigen::Vector3f kd = texture_color / 255.f;
    Eigen::Vector3f ks = Eigen::Vector3f(0.7937, 0.7937, 0.7937);

    auto l1 = light{{20, 20, 20}, {500, 500, 500}};
    auto l2 = light{{-20, 20, 0}, {500, 500, 500}};

    std::vector<light> lights = {l1, l2};
    Eigen::Vector3f amb_light_intensity{10, 10, 10};
    Eigen::Vector3f eye_pos{0, 0, 10};

    float p = 150;

    Eigen::Vector3f color = texture_color;
    Eigen::Vector3f point = payload.view_pos;
    Eigen::Vector3f normal = payload.normal;

    Eigen::Vector3f result_color = {0, 0, 0};

    for (auto &light : lights)
    {
        // TODO: For each light source in the code, calculate what the *ambient*, *diffuse*, and *specular*
        // components are. Then, accumulate that result on the *result_color* object.
        auto v = (eye_pos - point).normalized(); // 视线方向
        auto n = normal.normalized();
        auto l = (light.position - point).normalized();
        auto h = (v + l).normalized();
        auto atten = (light.position - point).dot(light.position - point); // 光线到物体表面的距离的平方
        auto ambient = ka.cwiseProduct(amb_light_intensity);
        auto diffuse = kd.cwiseProduct(light.intensity / atten) * std::max(0.0f, n.dot(l));
        auto specular = ks.cwiseProduct(light.intensity / atten) * std::pow(std::max(0.0f, n.dot(h)), p);
        result_color += specular + ambient + diffuse;
    }

    return result_color * 255.f;
}

Eigen::Vector3f phong_fragment_shader(const fragment_shader_payload &payload)
{
    Eigen::Vector3f ka = Eigen::Vector3f(0.005, 0.005, 0.005);    // 环境光的颜色
    Eigen::Vector3f kd = payload.color;                           // 漫反射的颜色
    Eigen::Vector3f ks = Eigen::Vector3f(0.7937, 0.7937, 0.7937); // 镜面反射的颜色

    auto l1 = light{{20, 20, 20}, {500, 500, 500}};
    auto l2 = light{{-20, 20, 0}, {500, 500, 500}};

    std::vector<light> lights = {l1, l2};            // 光源位置和强度
    Eigen::Vector3f amb_light_intensity{10, 10, 10}; // 环境光强度
    Eigen::Vector3f eye_pos{0, 0, 10};               // 视角位置

    float p = 200;

    Eigen::Vector3f color = payload.color;    // 片元传入颜色
    Eigen::Vector3f point = payload.view_pos; // 片元传入顶点位置
    Eigen::Vector3f normal = payload.normal;  // 片元传入法线

    Eigen::Vector3f result_color = {0, 0, 0}; // 最终颜色
    for (auto &light : lights)
    {
        // TODO: For each light source in the code, calculate what the *ambient*, *diffuse*, and *specular*
        // components are. Then, accumulate that result on the *result_color* object.
        // n,v,l,r
        auto v = (eye_pos - point).normalized(); // 视线方向
        auto n = normal.normalized();
        auto l = (light.position - point).normalized();
        auto h = (v + l).normalized();
        auto atten = (light.position - point).dot(light.position - point); // 光线到物体表面的距离的平方
        // auto ambient = ka * amb_light_intensity;//没有定义吗？
        auto ambient = ka.cwiseProduct(amb_light_intensity);
        auto diffuse = kd.cwiseProduct(light.intensity / atten) * std::max(0.0f, n.dot(l));
        auto specular = ks.cwiseProduct(light.intensity / atten) * std::pow(std::max(0.0f, n.dot(h)), p);
        result_color += specular + ambient + diffuse;
    }

    return result_color * 255.f;
}

Eigen::Vector3f displacement_fragment_shader(const fragment_shader_payload &payload)
{

    Eigen::Vector3f ka = Eigen::Vector3f(0.005, 0.005, 0.005);
    Eigen::Vector3f kd = payload.color;
    Eigen::Vector3f ks = Eigen::Vector3f(0.7937, 0.7937, 0.7937);

    auto l1 = light{{20, 20, 20}, {500, 500, 500}};
    auto l2 = light{{-20, 20, 0}, {500, 500, 500}};

    std::vector<light> lights = {l1, l2};
    Eigen::Vector3f amb_light_intensity{10, 10, 10};
    Eigen::Vector3f eye_pos{0, 0, 10};

    float p = 150;

    Eigen::Vector3f color = payload.color;
    Eigen::Vector3f point = payload.view_pos;
    Eigen::Vector3f normal = payload.normal;

    float kh = 0.2, kn = 0.1;

    // TODO: Implement displacement mapping here
    // Let n = normal = (x, y, z)
    // Vector t = (x*y/sqrt(x*x+z*z),sqrt(x*x+z*z),z*y/sqrt(x*x+z*z))
    // Vector b = n cross product t
    // Matrix TBN = [t b n]
    // dU = kh * kn * (h(u+1/w,v)-h(u,v))
    // dV = kh * kn * (h(u,v+1/h)-h(u,v))
    // Vector ln = (-dU, -dV, 1)
    // Position p = p + kn * n * h(u,v)
    // Normal n = normalize(TBN * ln)

    Eigen::Vector3f n = normal.normalized();
    float x = n.x();
    float y = n.y();
    float z = n.z();
    Eigen::Vector3f t, b;
    t << (x * y / std::sqrt(x * x + z * z)), std::sqrt(x * x + z * z), z * y / std::sqrt(x * x + z * z);
    b = n.cross(t);
    Eigen::Matrix3f TBN = Eigen::Matrix3f::Zero();
    TBN << t, b, n;
    float u = payload.tex_coords.x();
    float v = payload.tex_coords.y();
    float w = payload.texture->width;
    float h = payload.texture->height;
    float du = kh * kn * (payload.texture->getColor(u + 1.0 / w, v).norm() - payload.texture->getColor(u, v).norm());
    float dv = kh * kn * (payload.texture->getColor(u, v + 1.0 / h).norm() - payload.texture->getColor(u, v).norm());
    Eigen::Vector3f ln;
    ln << -du, -dv, 1;
    Eigen::Vector3f displacement_normal = (TBN * ln).normalized();
    point += kn * displacement_normal * payload.texture->getColor(u, v).norm();

    Eigen::Vector3f result_color = {0, 0, 0};

    for (auto &light : lights)
    {
        // TODO: For each light source in the code, calculate what the *ambient*, *diffuse*, and *specular*
        // components are. Then, accumulate that result on the *result_color* object.
        auto v = (eye_pos - point).normalized(); // 视线方向
        auto n = normal.normalized();
        auto l = (light.position - point).normalized();
        auto h = (v + l).normalized();
        auto atten = (light.position - point).dot(light.position - point); // 光线到物体表面的距离的平方
        // auto ambient = ka * amb_light_intensity;//没有定义吗？
        auto ambient = ka.cwiseProduct(amb_light_intensity);
        auto diffuse = kd.cwiseProduct(light.intensity / atten) * std::max(0.0f, n.dot(l));
        auto specular = ks.cwiseProduct(light.intensity / atten) * std::pow(std::max(0.0f, n.dot(h)), p);
        result_color += specular + ambient + diffuse;
    }

    return result_color * 255.f;
}

Eigen::Vector3f bump_fragment_shader(const fragment_shader_payload &payload)
{

    Eigen::Vector3f ka = Eigen::Vector3f(0.005, 0.005, 0.005);
    Eigen::Vector3f kd = payload.color;
    Eigen::Vector3f ks = Eigen::Vector3f(0.7937, 0.7937, 0.7937);

    auto l1 = light{{20, 20, 20}, {500, 500, 500}};
    auto l2 = light{{-20, 20, 0}, {500, 500, 500}};

    std::vector<light> lights = {l1, l2};
    Eigen::Vector3f amb_light_intensity{10, 10, 10};
    Eigen::Vector3f eye_pos{0, 0, 10};

    float p = 150;

    Eigen::Vector3f color = payload.color;
    Eigen::Vector3f point = payload.view_pos;
    Eigen::Vector3f normal = payload.normal;

    float kh = 0.2, kn = 0.1; // 常量影响系数，也就是c1,c2

    // TODO: Implement bump mapping here
    // Let n = normal = (x, y, z)
    // Vector t = (x*y/sqrt(x*x+z*z),sqrt(x*x+z*z),z*y/sqrt(x*x+z*z))
    // Vector b = n cross product t
    // Matrix TBN = [t b n]
    // dU = kh * kn * (h(u+1/w,v)-h(u,v))_h(u,v) = texture_color(u,v).norm
    // dV = kh * kn * (h(u,v+1/h)-h(u,v))
    // Vector ln = (-dU, -dV, 1)
    // Normal n = normalize(TBN * ln)

    /*一步步的拆分：
      1.kh*kn就不解释了，上面有说
      2.payload是所写函数输入的一个struct，这个struct包含了texture等信息，这个结构是在shader.hpp中定义的
      3.payload.texture ———— 取payload这个struct里的texture
        texture ———— 是Texture.hpp中定义的一个class，里面包含了纹理的宽高(width/height)和getColor()函数等信息
        payload.texture->getColor() ———— 访问到定义的这个getColor()函数
      4.为什么要用“u+1.0f/w”而不是直接“u+1”？我们仔细看Texture.hpp对getColor()的一段定义：
        ...
        auto u_img = u * width;
        auto v_img = (1 - v) * height;
        auto color = image_data.at<cv::Vec3b>(v_img, u_img);
        return Eigen::Vector3f(color[0], color[1], color[2]);
        ...
        这里的u v值都×了纹理对应的宽高，变换过来的话移动一个单位应该是“u*width+1”因此在我们的函数里1个单位对应的应该是1/width，1/h同理
      5.getColor().norm() ———— .norm()是Eigen库里定义的一个求范数的函数,就是求所有元素²的和再开方。
        向量的范数则表示的是原有集合的大小，范数的本质是距离，存在的意义是为了实现比较。
        这部分为什么要给个norm，我的理解是：getColor返回的是一个储存颜色值的向量：（color[0]，color[1]，color[2]）对应的是RGB值
        dU和dV都是一个float值，并不是Vector，想要实现h()表示的实数高度值，就要用到norm.()将向量映射成实数（个人理解，不确定对不对）
      6.还需要注意，这里的dU和dV对应的是老师课上给的dp/du和dp/dv
    */

    Eigen::Vector3f n = normal.normalized();
    float x = n.x();
    float y = n.y();
    float z = n.z();
    Eigen::Vector3f t, b;
    t << (x * y / std::sqrt(x * x + z * z)), std::sqrt(x * x + z * z), z * y / std::sqrt(x * x + z * z);
    b = n.cross(t);
    Eigen::Matrix3f TBN = Eigen::Matrix3f::Zero();
    TBN << t, b, n;
    float u = payload.tex_coords.x();
    float v = payload.tex_coords.y();
    float w = payload.texture->width;
    float h = payload.texture->height;
    float du = kh * kn * (payload.texture->getColor(u + 1.0 / w, v).norm() - payload.texture->getColor(u, v).norm());
    float dv = kh * kn * (payload.texture->getColor(u, v + 1.0 / h).norm() - payload.texture->getColor(u, v).norm());
    Eigen::Vector3f ln;
    ln << -du, -dv, 1;
    Eigen::Vector3f bump_normal = (TBN * ln).normalized();

    Eigen::Vector3f result_color = {0, 0, 0};
    result_color = bump_normal;

    return result_color * 255.f;
}

int main(int argc, const char **argv)
{
    std::vector<Triangle *> TriangleList;

    float angle = 140.0;
    bool command_line = false;

    std::string filename = "output.png";
    objl::Loader Loader;
    std::string obj_path = "../models/spot/";

    // Load .obj File
    bool loadout = Loader.LoadFile("../models/spot/spot_triangulated_good.obj");
    for (auto mesh : Loader.LoadedMeshes)
    {
        for (int i = 0; i < mesh.Vertices.size(); i += 3)
        {
            Triangle *t = new Triangle();
            for (int j = 0; j < 3; j++)
            {
                t->setVertex(j, Vector4f(mesh.Vertices[i + j].Position.X, mesh.Vertices[i + j].Position.Y, mesh.Vertices[i + j].Position.Z, 1.0));
                t->setNormal(j, Vector3f(mesh.Vertices[i + j].Normal.X, mesh.Vertices[i + j].Normal.Y, mesh.Vertices[i + j].Normal.Z));
                t->setTexCoord(j, Vector2f(mesh.Vertices[i + j].TextureCoordinate.X, mesh.Vertices[i + j].TextureCoordinate.Y));
            }
            TriangleList.push_back(t);
        }
    }

    rst::rasterizer r(700, 700);

    auto texture_path = "hmap.jpg";
    r.set_texture(Texture(obj_path + texture_path));

    std::function<Eigen::Vector3f(fragment_shader_payload)> active_shader = phong_fragment_shader;

    if (argc >= 2)
    {
        command_line = true;
        filename = std::string(argv[1]);

        if (argc == 3 && std::string(argv[2]) == "texture")
        {
            std::cout << "Rasterizing using the texture shader\n";
            active_shader = texture_fragment_shader;
            texture_path = "spot_texture.png";
            r.set_texture(Texture(obj_path + texture_path));
        }
        else if (argc == 3 && std::string(argv[2]) == "normal")
        {
            std::cout << "Rasterizing using the normal shader\n";
            active_shader = normal_fragment_shader;
        }
        else if (argc == 3 && std::string(argv[2]) == "phong")
        {
            std::cout << "Rasterizing using the phong shader\n";
            active_shader = phong_fragment_shader;
        }
        else if (argc == 3 && std::string(argv[2]) == "bump")
        {
            std::cout << "Rasterizing using the bump shader\n";
            active_shader = bump_fragment_shader;
        }
        else if (argc == 3 && std::string(argv[2]) == "displacement")
        {
            std::cout << "Rasterizing using the bump shader\n";
            active_shader = displacement_fragment_shader;
        }
    }

    Eigen::Vector3f eye_pos = {0, 0, 10};

    r.set_vertex_shader(vertex_shader);
    r.set_fragment_shader(active_shader);

    int key = 0;
    int frame_count = 0;

    if (command_line)
    {
        r.clear(rst::Buffers::Color | rst::Buffers::Depth);
        r.set_model(get_model_matrix(angle));
        r.set_view(get_view_matrix(eye_pos));
        r.set_projection(get_projection_matrix(45.0, 1, 0.1, 50));

        r.draw(TriangleList);
        cv::Mat image(700, 700, CV_32FC3, r.frame_buffer().data());
        image.convertTo(image, CV_8UC3, 1.0f);
        cv::cvtColor(image, image, cv::COLOR_RGB2BGR);

        cv::imwrite(filename, image);

        return 0;
    }

    while (key != 27)
    {
        r.clear(rst::Buffers::Color | rst::Buffers::Depth);

        r.set_model(get_model_matrix(angle));
        r.set_view(get_view_matrix(eye_pos));
        r.set_projection(get_projection_matrix(45.0, 1, 0.1, 50));

        // r.draw(pos_id, ind_id, col_id, rst::Primitive::Triangle);
        r.draw(TriangleList);
        cv::Mat image(700, 700, CV_32FC3, r.frame_buffer().data());
        image.convertTo(image, CV_8UC3, 1.0f);
        cv::cvtColor(image, image, cv::COLOR_RGB2BGR);

        cv::imshow("image", image);
        cv::imwrite(filename, image);
        key = cv::waitKey(10);

        if (key == 'a')
        {
            angle -= 0.1;
        }
        else if (key == 'd')
        {
            angle += 0.1;
        }
    }
    return 0;
}

```



rasterizer的代码：

```C++
//
// Created by goksu on 4/6/19.
//

#include <algorithm>
#include "rasterizer.hpp"
#include <opencv2/opencv.hpp>
#include <math.h>

rst::pos_buf_id rst::rasterizer::load_positions(const std::vector<Eigen::Vector3f> &positions)
{
    auto id = get_next_id();
    pos_buf.emplace(id, positions);

    return {id};
}

rst::ind_buf_id rst::rasterizer::load_indices(const std::vector<Eigen::Vector3i> &indices)
{
    auto id = get_next_id();
    ind_buf.emplace(id, indices);

    return {id};
}

rst::col_buf_id rst::rasterizer::load_colors(const std::vector<Eigen::Vector3f> &cols)
{
    auto id = get_next_id();
    col_buf.emplace(id, cols);

    return {id};
}

rst::col_buf_id rst::rasterizer::load_normals(const std::vector<Eigen::Vector3f> &normals)
{
    auto id = get_next_id();
    nor_buf.emplace(id, normals);

    normal_id = id;

    return {id};
}

// Bresenham's line drawing algorithm
void rst::rasterizer::draw_line(Eigen::Vector3f begin, Eigen::Vector3f end)
{
    auto x1 = begin.x();
    auto y1 = begin.y();
    auto x2 = end.x();
    auto y2 = end.y();

    Eigen::Vector3f line_color = {255, 255, 255};

    int x, y, dx, dy, dx1, dy1, px, py, xe, ye, i;

    dx = x2 - x1;
    dy = y2 - y1;
    dx1 = fabs(dx);
    dy1 = fabs(dy);
    px = 2 * dy1 - dx1;
    py = 2 * dx1 - dy1;

    if (dy1 <= dx1)
    {
        if (dx >= 0)
        {
            x = x1;
            y = y1;
            xe = x2;
        }
        else
        {
            x = x2;
            y = y2;
            xe = x1;
        }
        Eigen::Vector2i point = Eigen::Vector2i(x, y);
        set_pixel(point, line_color);
        for (i = 0; x < xe; i++)
        {
            x = x + 1;
            if (px < 0)
            {
                px = px + 2 * dy1;
            }
            else
            {
                if ((dx < 0 && dy < 0) || (dx > 0 && dy > 0))
                {
                    y = y + 1;
                }
                else
                {
                    y = y - 1;
                }
                px = px + 2 * (dy1 - dx1);
            }
            //            delay(0);
            Eigen::Vector2i point = Eigen::Vector2i(x, y);
            set_pixel(point, line_color);
        }
    }
    else
    {
        if (dy >= 0)
        {
            x = x1;
            y = y1;
            ye = y2;
        }
        else
        {
            x = x2;
            y = y2;
            ye = y1;
        }
        Eigen::Vector2i point = Eigen::Vector2i(x, y);
        set_pixel(point, line_color);
        for (i = 0; y < ye; i++)
        {
            y = y + 1;
            if (py <= 0)
            {
                py = py + 2 * dx1;
            }
            else
            {
                if ((dx < 0 && dy < 0) || (dx > 0 && dy > 0))
                {
                    x = x + 1;
                }
                else
                {
                    x = x - 1;
                }
                py = py + 2 * (dx1 - dy1);
            }
            //            delay(0);
            Eigen::Vector2i point = Eigen::Vector2i(x, y);
            set_pixel(point, line_color);
        }
    }
}

auto to_vec4(const Eigen::Vector3f &v3, float w = 1.0f)
{
    return Vector4f(v3.x(), v3.y(), v3.z(), w);
}

static bool insideTriangle(int x, int y, const Vector4f *_v)
{
    Vector3f v[3];
    for (int i = 0; i < 3; i++)
        v[i] = {_v[i].x(), _v[i].y(), 1.0};
    Vector3f f0, f1, f2;
    f0 = v[1].cross(v[0]);
    f1 = v[2].cross(v[1]);
    f2 = v[0].cross(v[2]);
    Vector3f p(x, y, 1.);
    if ((p.dot(f0) * f0.dot(v[2]) > 0) && (p.dot(f1) * f1.dot(v[0]) > 0) && (p.dot(f2) * f2.dot(v[1]) > 0))
        return true;
    return false;
}

static std::tuple<float, float, float> computeBarycentric2D(float x, float y, const Vector4f *v)
{
    float c1 = (x * (v[1].y() - v[2].y()) + (v[2].x() - v[1].x()) * y + v[1].x() * v[2].y() - v[2].x() * v[1].y()) / (v[0].x() * (v[1].y() - v[2].y()) + (v[2].x() - v[1].x()) * v[0].y() + v[1].x() * v[2].y() - v[2].x() * v[1].y());
    float c2 = (x * (v[2].y() - v[0].y()) + (v[0].x() - v[2].x()) * y + v[2].x() * v[0].y() - v[0].x() * v[2].y()) / (v[1].x() * (v[2].y() - v[0].y()) + (v[0].x() - v[2].x()) * v[1].y() + v[2].x() * v[0].y() - v[0].x() * v[2].y());
    float c3 = (x * (v[0].y() - v[1].y()) + (v[1].x() - v[0].x()) * y + v[0].x() * v[1].y() - v[1].x() * v[0].y()) / (v[2].x() * (v[0].y() - v[1].y()) + (v[1].x() - v[0].x()) * v[2].y() + v[0].x() * v[1].y() - v[1].x() * v[0].y());
    return {c1, c2, c3};
}

void rst::rasterizer::draw(std::vector<Triangle *> &TriangleList)
{

    float f1 = (50 - 0.1) / 2.0;
    float f2 = (50 + 0.1) / 2.0;

    Eigen::Matrix4f mvp = projection * view * model;
    for (const auto &t : TriangleList) //&t的意思是引用，这里的t是Triangle的指针，针对的是每一个三角形
    {
        Triangle newtri = *t; //*t是拷贝，这里的t是Triangle的指针，所以这里的newtri是拷贝的Triangle

        std::array<Eigen::Vector4f, 3> mm{
            (view * model * t->v[0]),
            (view * model * t->v[1]),
            (view * model * t->v[2])};

        std::array<Eigen::Vector3f, 3> viewspace_pos;

        std::transform(mm.begin(), mm.end(), viewspace_pos.begin(), [](auto &v)
                       { return v.template head<3>(); });

        Eigen::Vector4f v[] = {
            mvp * t->v[0],
            mvp * t->v[1],
            mvp * t->v[2]};
        // Homogeneous division
        for (auto &vec : v)
        {
            vec.x() /= vec.w();
            vec.y() /= vec.w();
            vec.z() /= vec.w();
        }

        Eigen::Matrix4f inv_trans = (view * model).inverse().transpose();
        Eigen::Vector4f n[] = {
            inv_trans * to_vec4(t->normal[0], 0.0f),
            inv_trans * to_vec4(t->normal[1], 0.0f),
            inv_trans * to_vec4(t->normal[2], 0.0f)};

        // Viewport transformation
        for (auto &vert : v)
        {
            vert.x() = 0.5 * width * (vert.x() + 1.0);
            vert.y() = 0.5 * height * (vert.y() + 1.0);
            vert.z() = vert.z() * f1 + f2;
        }

        for (int i = 0; i < 3; ++i)
        {
            // screen space coordinates
            newtri.setVertex(i, v[i]);
        }

        for (int i = 0; i < 3; ++i)
        {
            // view space normal
            newtri.setNormal(i, n[i].head<3>());
        }

        newtri.setColor(0, 148, 121.0, 92.0);
        newtri.setColor(1, 148, 121.0, 92.0);
        newtri.setColor(2, 148, 121.0, 92.0);

        // Also pass view space vertice position
        rasterize_triangle(newtri, viewspace_pos);
    }
}

static Eigen::Vector3f interpolate(float alpha, float beta, float gamma, const Eigen::Vector3f &vert1, const Eigen::Vector3f &vert2, const Eigen::Vector3f &vert3, float weight)
// weight传入的是三个顶点对w的加权平均
{
    return (alpha * vert1 + beta * vert2 + gamma * vert3) / weight;
}

static Eigen::Vector2f interpolate(float alpha, float beta, float gamma, const Eigen::Vector2f &vert1, const Eigen::Vector2f &vert2, const Eigen::Vector2f &vert3, float weight)
{
    auto u = (alpha * vert1[0] + beta * vert2[0] + gamma * vert3[0]);
    auto v = (alpha * vert1[1] + beta * vert2[1] + gamma * vert3[1]);

    u /= weight;
    v /= weight;

    return Eigen::Vector2f(u, v);
}

// Screen space rasterization
void rst::rasterizer::rasterize_triangle(const Triangle &t, const std::array<Eigen::Vector3f, 3> &view_pos)
{
    auto v = t.toVector4();
    Vector3f color;
    float alpha, beta, gamma, lmin=INT_MAX, rmax=INT_MIN, tmax=INT_MIN, bmin=INT_MAX, id;
    for(auto &k:v){//找到bounding box的边界坐标
        lmin = int(std::min(lmin,k.x()));
        rmax = std::max(rmax,k.x());rmax = rmax == int(rmax) ? int(rmax)-1 : rmax;
        tmax = std::max(tmax,k.y());tmax = tmax == int(tmax) ? int(tmax)-1 : tmax;
        bmin = int(std::min(bmin,k.y()));
    }
    for(float i = lmin; i <= rmax; i++){
        for(float j = bmin; j <= tmax; j++){//遍历bounding box像素
            id = get_index(i,j);
            if(insideTriangle(i+0.5, j+0.5, t.v)){//如果像素在三角形内
                // If so, use the following code to get the interpolated z value.
                std::tie(alpha, beta, gamma) = computeBarycentric2D(i+0.5, j+0.5, t.v);
                float w_reciprocal = 1.0/(alpha / v[0].w() + beta / v[1].w() + gamma / v[2].w());
                float z_interpolated = alpha * v[0].z() / v[0].w() + beta * v[1].z() / v[1].w() + gamma * v[2].z() / v[2].w();
                z_interpolated *= w_reciprocal;
                    
                if (z_interpolated < depth_buf[id]){//如果该像素的深度更小，更新像素深度、颜色表
                    auto interpolated_color = interpolate(alpha, beta, gamma, t.color[0], t.color[1], t.color[2], 1);//颜色插值
                    auto interpolated_normal = interpolate(alpha, beta, gamma, t.normal[0], t.normal[1], t.normal[2], 1).normalized();//法向量插值
                    auto interpolated_texcoords = interpolate(alpha, beta, gamma, t.tex_coords[0], t.tex_coords[1], t.tex_coords[2], 1);//纹理坐标插值
                    auto interpolated_shadingcoords = interpolate(alpha, beta, gamma, view_pos[0], view_pos[1], view_pos[2], 1);//着色点坐标插值
                    fragment_shader_payload payload(interpolated_color, interpolated_normal.normalized(), interpolated_texcoords, texture ? &*texture : nullptr);//将插值属性传入fragment_shader_payload
                    payload.view_pos = interpolated_shadingcoords;//传入原顶点坐标
                    depth_buf[id] = z_interpolated;    
                    frame_buf[id] = fragment_shader(payload);//使用shader计算颜色
                    // TODO : set the current pixel (use the set_pixel function) to the color of the triangle (use getColor function) if it should be painted.
                    set_pixel({i,j}, frame_buf[id]);
                }
            }
        }
    } 
}

void rst::rasterizer::set_model(const Eigen::Matrix4f &m)
{
    model = m;
}

void rst::rasterizer::set_view(const Eigen::Matrix4f &v)
{
    view = v;
}

void rst::rasterizer::set_projection(const Eigen::Matrix4f &p)
{
    projection = p;
}

void rst::rasterizer::clear(rst::Buffers buff)
{
    if ((buff & rst::Buffers::Color) == rst::Buffers::Color)
    {
        std::fill(frame_buf.begin(), frame_buf.end(), Eigen::Vector3f{0, 0, 0});
        std::fill(super_sample_buf.begin(), super_sample_buf.end(), Eigen::Vector3f{0, 0, 0});
    }
    if ((buff & rst::Buffers::Depth) == rst::Buffers::Depth)
    {
        std::fill(depth_buf.begin(), depth_buf.end(), std::numeric_limits<float>::infinity());
        std::fill(super_sample_depth_buf.begin(), super_sample_depth_buf.end(), std::numeric_limits<float>::infinity());
    }
}

rst::rasterizer::rasterizer(int w, int h) : width(w), height(h)
{
    frame_buf.resize(w * h);
    depth_buf.resize(w * h);

    super_sample_buf.resize(w * h * super_sample_count);       // 超采样颜色缓冲区扩充
    super_sample_depth_buf.resize(w * h * super_sample_count); // 超采样深度缓冲区扩充

    texture = std::nullopt;
}

int rst::rasterizer::get_index(int x, int y)
{
    return (height - y) * width + x;
}

void rst::rasterizer::set_pixel(const Vector2i &point, const Eigen::Vector3f &color)
{
    // old index: auto ind = point.y() + point.x() * width;
    int ind = (height - point.y()) * width + point.x();
    frame_buf[ind] = color;
}

void rst::rasterizer::set_vertex_shader(std::function<Eigen::Vector3f(vertex_shader_payload)> vert_shader)
{
    vertex_shader = vert_shader;
    // vertex_shader里面有position
}

void rst::rasterizer::set_fragment_shader(std::function<Eigen::Vector3f(fragment_shader_payload)> frag_shader)
{
    fragment_shader = frag_shader;
    // fragment_shader里面有color, normal, texcoords, view_pos,Texture* texture
}

```



## Homework4

确实有好有点，但感觉还不够

![image-20241014161117633](D:\TyporaImage\image-20241014161117633.png)



来一个双线性插值试试看

![image-20241014165454279](D:\TyporaImage\image-20241014165454279.png)

```C++
#include <chrono>
#include <iostream>
#include <opencv2/opencv.hpp>

std::vector<cv::Point2f> control_points;

void mouse_handler(int event, int x, int y, int flags, void *userdata)
// 鼠标单击创造点，点数控制4
{
    if (event == cv::EVENT_LBUTTONDOWN && control_points.size() < 4)
    {
        std::cout << "Left button of the mouse is clicked - position (" << x << ", "
                  << y << ")" << '\n';
        control_points.emplace_back(x, y); // 直接构造点，省去临时创建对象再赋予，比push_back快
    }
}

void naive_bezier(const std::vector<cv::Point2f> &points, cv::Mat &window)
// 三阶贝塞尔曲线绘制，着色红
{
    auto &p_0 = points[0]; // points存储了二维点的坐标
    auto &p_1 = points[1];
    auto &p_2 = points[2];
    auto &p_3 = points[3];

    for (double t = 0.0; t <= 1.0; t += 0.001)
    {
        auto point = std::pow(1 - t, 3) * p_0 + 3 * t * std::pow(1 - t, 2) * p_1 + 3 * std::pow(t, 2) * (1 - t) * p_2 + std::pow(t, 3) * p_3;

        window.at<cv::Vec3b>(point.y, point.x)[2] = 255; // 对Y行X列的像素点的B着色——BGR
    }
}

cv::Point2f recursive_bezier(const std::vector<cv::Point2f> &control_points, float t)
{
    // TODO: Implement de Casteljau's algorithm
    if (control_points.size() == 1)
    {
        return control_points[0];
    } // 归

    std::vector<cv::Point2f> next_control_points;
    for (int i = 0; i < control_points.size() - 1; i++)
    {
        auto &a = control_points[i];     // 把control_points[i]的地址给&a，避免复制操作
        auto &b = control_points[i + 1]; // 把control_points[i+1]的地址给&b
        auto p = (1 - t) * a + t * b;
        next_control_points.push_back(p);
    }

    return recursive_bezier(next_control_points, t);
}

void bezier(const std::vector<cv::Point2f> &control_points, cv::Mat &window)
{
    // TODO: Iterate through all t = 0 to t = 1 with small steps, and call de Casteljau's
    // recursive Bezier algorithm.
    for (double t = 0.0; t <= 1.0; t += 0.001)
    {
        auto point = recursive_bezier(control_points, t);

        auto centerPoint = cv ::Point2f(int(point.x) + 0.5f, int(point.y) + 0.5f);

        auto x = point.x, y = point.y, centerX = centerPoint.x, centerY = centerPoint.y;
        // x,y是曲线上的点，centerX,centerY是像素上的点
        for (int i = -1; i <= 1; i++)
        {
            for (int j = -1; j <= 1; j++)
            {
                float distance = std::pow(x - (centerX - i), 2) + std::pow(y - (centerY - j), 2);
                float normalize_distance = std::sqrt(2) / 3; // 距离映射到[0,1]
                float color = 255 * (1 - normalize_distance);

                if (window.at<cv::Vec3b>(point.y - j, point.x - i)[2] < color)
                    window.at<cv::Vec3b>(point.y - j, point.x - i)[2] = color; // 对Y行X列的像素点的B着色——BGR
            }
        }
    }

    // 双线性插值：https://zhuanlan.zhihu.com/p/464122963
    // 我看不懂t和s是怎么来的

    cv::Mat window_copy = window.clone();

    float min_x = window.cols;
    float max_x = 0;
    float min_y = window.rows;
    float max_y = 0;

    // find out the bounding box of current line
    for (int i = 0; i < control_points.size(); i++)
    {
        min_x = std::min(control_points[i].x, min_x);
        max_x = std::max(control_points[i].x, max_x);
        min_y = std::min(control_points[i].y, min_y);
        max_y = std::max(control_points[i].y, max_y);
    }
    for (int y = min_y; y < max_y; y++)
    {
        for (int x = min_x; x < max_x; x++)
        {

            for (float j = 0.25; j < 1.; j += 0.5)
            {
                for (float i = 0.25; i < 1.; i += 0.5)
                {

                    // find center coordinates
                    int cx = i > 0.5 ? x + 1 : x;
                    int cy = j > 0.5 ? y + 1 : y;
                    if (cx > max_x || cy > max_y)
                        continue;

                    cv::Vec3b u00 = window_copy.at<cv::Vec3b>(cy - 0.5, cx - 0.5);
                    cv::Vec3b u10 = window_copy.at<cv::Vec3b>(cy - 0.5, cx + 0.5);
                    cv::Vec3b u01 = window_copy.at<cv::Vec3b>(cy + 0.5, cx - 0.5);
                    cv::Vec3b u11 = window_copy.at<cv::Vec3b>(cy + 0.5, cx + 0.5);

                    float s = (x + i) - (cx - 0.5); // 我一直搞不清楚这个插值系数到底是怎么来的
                    float t = (y + j) - (cy - 0.5);

                    cv::Vec3b u0 = (1 - s) * u00 + s * u10;
                    cv::Vec3b u1 = (1 - s) * u01 + s * u11;
                    cv::Vec3b res = (1 - t) * u0 + t * u1;

                    window.at<cv::Vec3b>(y, x)[0] = res[0];
                    window.at<cv::Vec3b>(y, x)[1] = res[1];
                    window.at<cv::Vec3b>(y, x)[2] = res[2];
                }
            }
        }
    }
}

int main()
{
    cv::Mat window = cv::Mat(700, 700, CV_8UC3, cv::Scalar(0));
    cv::cvtColor(window, window, cv::COLOR_BGR2RGB);
    cv::namedWindow("Bezier Curve", cv::WINDOW_AUTOSIZE);

    cv::setMouseCallback("Bezier Curve", mouse_handler, nullptr); // 回调时，名字，回调方法，以及回调时要传入void *userdata的参数

    int key = -1;
    while (key != 27)
    {
        for (auto &point : control_points) // 画出控制点
        {
            cv::circle(window, point, 3, {255, 255, 255}, 3);
        }

        if (control_points.size() == 4) // 画出曲线
        {
            // naive_bezier(control_points, window);
            bezier(control_points, window);

            cv::imshow("Bezier Curve", window);
            cv::imwrite("my_bezier_curve.png", window);
            key = cv::waitKey(0);

            return 0;
        }

        cv::imshow("Bezier Curve", window);
        key = cv::waitKey(20);
    }

    return 0;
}

```







## Homework5

![binary](D:\TyporaImage\binary.jpg)



## Homework6

![image-20241019234423243](D:\TyporaImage\image-20241019234423243.png)

![binary](D:\TyporaImage\binary.png)



## 提高作业：

### SAH

实际上它是个分割策略，是**BVH的加速结构**。当找到最佳的分割点后，需要再次递归调用 BVH 加速结构。

- ### 原理

  - SAH涉及对时间成本的估计，以面积来作为概率，对此进行概率与时间成本消耗（怎么感觉有点像是负面期望）	

- ### 涵盖内容

  - 将总体的bound的面积考虑进去了，考虑了单个图元（三角形）在某个bound的面积占比——面积比代认为为概率

- ### 开销成本

  - 对单个图元被遍历到进行单次时间成本的期望计算，对同个盒子里面的三角形进行了全图元期望累加，

- ### 表现

  - 表现为——图元密集的区域划分面积少（仍然是轴向划分），图元稀疏区域面积大。图元密集区的判定框自然是越小越好，反之越大越好。

- ### 逻辑与思考

  - 逻辑上讲，图元小的区域，被射线穿中的可能性本身就小。如果探测面积大，那自然意味着更可能射中少图元的bound，加上其本身就很小的性质，被光线打中的可能性就更小了，时间探测开销也小；如果区域图元密集，假如探测区域大，被射线打中盒子的可能性就大，如果射线打中了，那就得被迫对密集的所有图元都进行一次遍历，判定是否与射线相交。射线击中后需要检查更多的图元，这会增加探测的时间成本。图元密集区域自然是希望判定盒越小越好，省的那个麻烦全遍历一次。

- ### 总

  - 所以，SVH 的核心思想是在划分时对区域进行更细化的调整：对于包含更多图元的区域，划分得更加精细，以减少射线击中后遍历多余图元的时间开销；而对于图元较少的区域，可以适当增加划分的边界面积，以降低射线击中后进入进一步检测的概率，从而整体上优化了探测的效率。

- ### 计算方法

  - 有实验说在12个物体之前仍然保持对半分会比SAH快一点
  - 以**节点**为单位，进行盒子的开销计算。根据开销，进行盒子细分。
  - ![image-20241020002049788](D:\TyporaImage\image-20241020002049788.png)
  - *C(A, B) 光线与这个**节点**相交的成本*
  - *t_traversal 是光线与中间节点相交的成本*——**自定义，一般为2（测试得出）**
  - *p_A* and *p_B* 是光线通过包围盒A与包围盒B的概率
  - *N_A* and *N_B* 是包围盒A和包围盒B中物体数量
  - *t_intersect* 是对一个物体的光线相交计算成本——**一般为1**

  - ​	![image-20241020002209520](D:\TyporaImage\image-20241020002209520.png)



肉眼上可以看到变快，但是数据上看不出来，留着给下一节用吧

![image-20241020012951677](D:\TyporaImage\image-20241020012951677.png)



- ### 参考

  - https://zhuanlan.zhihu.com/p/477316706
  - https://blog.csdn.net/ycrsw/article/details/124331686



## Homework7

参考：

- https://blog.csdn.net/qq_41835314/article/details/125166417

- https://blog.csdn.net/Q_pril/article/details/124206795

伪代码：

至少得判断完光线是否与场景/灯光/物体相交完再作这个代码

```C++
shade (p, wo)
    sampleLight ( inter , pdf_light )
    Get x, ws , NN , emit from inter
    Shoot a ray from p to x
    If the ray is not blocked in the middle
        L_dir = emit * eval(wo , ws , N) * dot(ws , N) * dot(ws , NN) / |x-p|^2 / pdf_light
    
    L_indir = 0.0
    Test Russian Roulette with probability RussianRoulette
    wi = sample (wo , N)
    Trace a ray r(p, wi)
    If ray r hit a non - emitting object at q
        L_indir = shade (q, -wi) * eval (wo , wi , N) * dot(wi , N) / pdf(wo , wi , N) / RussianRoulette
    
    Return L_dir + L_indir
```





用BVH和用了SAH加速结构，结果只差1秒……

![image-20241021150246732](D:\TyporaImage\image-20241021150246732.png)

![image-20241021150235755](D:\TyporaImage\image-20241021150235755.png)



SPP=2

![binary](D:\TyporaImage\binary-1729494265745-1.png)



![image-20241021152647926](D:\TyporaImage\image-20241021152647926.png)

![binary](D:\TyporaImage\binary-1729495616426-3.png)

为什么有一半会在阴影里面的？

我提高了判定盒子的容差，但似乎不起作用

他的阴影还像是沿着三角形网格分布的



![binary](D:\TyporaImage\binary-1729496193795-5.png)



修改了对于阴影区域的判断逻辑，定位问题就出现在对于阴影的判断逻辑上

![binary](D:\TyporaImage\binary-1729496998775-7.jpg)



```C++
if (light_to_obj.happened && (light_to_obj.distance - dis > -0.01f))
    // 如果没有光线遮挡_如果修改光线遮挡逻辑
    // 确实会加深光线对阴影区域的判断，那问题就是出在判断逻辑里面
    // 把容差从Epsilon提高宽容度就可以了
```

SPP=2

![binary](D:\TyporaImage\binary-1729498160157-9.jpg)





SPP=4

![binary](D:\TyporaImage\binary-1729498302992-11.jpg)

ssp=16

![binary](D:\TyporaImage\binary-1729498718580-13.jpg)

SSP=128

![binary](D:\TyporaImage\binary-1729501197135-1.jpg)





SPP=256

![binary](D:\TyporaImage\binary-1729512541719-15.jpg)



SPP=512

花了近两个小时多

所仍然能够看到很多噪点，估计是pdf为均值采样所带来的负面影响

[GAMES101作业7及课程总结（重点实现多线程加速，微表面模型材质） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/606074595)

这里有提到要做**重要性采样**，不然SPP怎么拉高都会有很多噪点

“哦对了，**最后的最后，因为要做重要性采样，castRay函数也要进行修改**，否则会有过曝现象，mirror材质不再需要直接光L_dir采样了，就直接算L_indir就行，而且在算L_indir的时候记得那个把里面的避免光源采样的判断条件去了。如下：”

![binary](D:\TyporaImage\binary-1729523631914-19.jpg)

------

# GAMES202

## HW0

因为我下的是老版本的，我寻思也没人跟我说一下顶点着色器的代码改了……

视口矩阵要改

然后传给fragment的两个varying值应该直接使用从其他地方传过来的，改成

```javascript
void main(void) {
    vFragPos = aVertexPosition;
    vNormal = aNormalPosition;
    //vFragPos = (uModelMatrix * vec4(aVertexPosition, 1.0)).xyz;
    //vNormal = (uModelMatrix * vec4(aNormalPosition, 0.0)).xyz;

    gl_Position = uProjectionMatrix * uModelViewMatrix * vec4(aVertexPosition, 1.0);
    vTextureCoord = aTextureCoord;
}
```

![image-20241109203553919](D:\TyporaImage\image-20241109203553919.png)





## HW1

https://blog.csdn.net/qq_58622402/article/details/124276196——原理

https://howl144.github.io/2023/05/15/00015.%20Games202%20Hw1/——进阶

![image-20241110180817750](D:\TyporaImage\image-20241110180817750.png)



确实能够解决一些自遮挡问题，然后对于部分阴影裁切问题也确实引发了。

![image-20241110181839766](D:\TyporaImage\image-20241110181839766.png)

**PCF**

![image-20241111141632555](D:\TyporaImage\image-20241111141632555.png)



![image-20241111152918616](D:\TyporaImage\image-20241111152918616.png)

相似三角形，$w_{light}$实际上就是光照强度映射成长度了，就是↑右边那个$W_{light}$。

![image-20241111152836683](D:\TyporaImage\image-20241111152836683.png)

![image-20241111155456852](D:\TyporaImage\image-20241111155456852.png)



## HW2

https://github.com/DrFlower/GAMES_101_202_Homework/tree/main/Homework_202/Assignment2
