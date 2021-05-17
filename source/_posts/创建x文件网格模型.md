---
title: 创建x文件网格模型
date: 2020-07-13 10:41:44
tags:
    - DirectX
categories: DirectX
---

{% centerquote %} 由3dmax模型到DirectX模型 {% endcenterquote %}
<!-- more -->

# 思路简述
三维文本网格模型的绘制流程和茶壶模型绘制基本相同，在InitGeometry部分作出一定修改即可。

# InitGeometry
> 创建场景图形

```
HRESULT InitGeometry()
{
	HRESULT hr;

	LPD3DXBUFFER pD3DXMtrlBuffer;  //存储网格模型材质的缓存对象

	//从磁盘文件加载网格模型
	V_RETURN(D3DXLoadMeshFromX(L"plane.x", D3DXMESH_MANAGED, g_pd3dDevice, NULL,
		&pD3DXMtrlBuffer, NULL, &g_dwNumMaterials, &g_pMesh));

	//从网格模型中提取材质属性和纹理文件名 
	D3DXMATERIAL* d3dxMaterials = (D3DXMATERIAL*)pD3DXMtrlBuffer->GetBufferPointer();
	g_pMeshMaterials = new D3DMATERIAL9[g_dwNumMaterials];

	if (g_pMeshMaterials == NULL)
		return E_OUTOFMEMORY;

	g_pMeshTextures = new LPDIRECT3DTEXTURE9[g_dwNumMaterials];
	if (g_pMeshTextures == NULL)
		return E_OUTOFMEMORY;

	//逐块提取网格模型材质属性和纹理文件名
	for (DWORD i = 0; i < g_dwNumMaterials; i++)
	{
		//材料属性
		g_pMeshMaterials[i] = d3dxMaterials[i].MatD3D;

		//设置模型材料的环境光反射系数, 因为模型材料本身没有设置环境光反射系数
		g_pMeshMaterials[i].Ambient = g_pMeshMaterials[i].Diffuse;

		g_pMeshTextures[i] = NULL;
		if (d3dxMaterials[i].pTextureFilename != NULL &&
			strlen(d3dxMaterials[i].pTextureFilename) > 0)
		{
			//获取纹理文件名
			WCHAR filename[256];
			RemovePathFromFileName(d3dxMaterials[i].pTextureFilename, filename);

			//创建纹理
			if (FAILED(D3DXCreateTextureFromFile(g_pd3dDevice, filename,
				&g_pMeshTextures[i])))
			{
				MessageBox(NULL, L"没有找到纹理文件!", L"FileMesh", MB_OK);
			}
		}
	}

	//释放在加载模型文件时创建的保存模型材质和纹理数据的缓存对象
	pD3DXMtrlBuffer->Release();

	//为网格模型计算法线信息
	if (!(g_pMesh->GetFVF() & D3DFVF_NORMAL))
	{
		//克隆网格模型
		LPD3DXMESH pTempMesh = NULL;
		g_pMesh->CloneMeshFVF(D3DXMESH_MANAGED, g_pMesh->GetFVF() | D3DFVF_NORMAL, g_pd3dDevice, &pTempMesh);

		//为网格模型计算法线
		D3DXComputeNormals(pTempMesh, NULL);

		//释放原来的网格模型对象, 并保存新网格模型
		g_pMesh->Release();
		g_pMesh = pTempMesh;
	}

	return S_OK;
}
```

## D3DXLoadMeshFromX
> 从DirectX .x文件加载网格

### 函数原型
```
HRESULT D3DXLoadMeshFromX(
  _In_  LPCTSTR           pFilename,    \\ 指向指定文件名的字符串的指针
  _In_  DWORD             Options,      \\ D3DXMESH枚举中的一个或多个标志的组合，用于指定网格的创建选项
  _In_  LPDIRECT3DDEVICE9 pD3DDevice,   \\ 指向IDirect3DDevice9接口的指针，该接口是与网格关联的设备对象
  _Out_ LPD3DXBUFFER      *ppAdjacency, \\指向包含邻接数据的缓冲区的指针
  _Out_ LPD3DXBUFFER      *ppMaterials, \\ 指向包含物料数据的缓冲区的指针
  _Out_ LPD3DXBUFFER      *ppEffectInstances,   \\ 指向一个缓冲区的指针，该缓冲区包含一个效果实例数组
  _Out_ DWORD             *pNumMaterials,\\ 当方法返回时，指向ppMaterials数组中D3DXMATERIAL结构的数量的指针   
  _Out_ LPD3DXMESH        *ppMesh         \\ 指向ID3DXMesh接口的指针的地址，表示加载的网格
);
```

#### ID3DXBuffer
> ID3DXBuffer接口是一个通用的数据缓存，D3DX用它来保存一块连续内存区域中的数据，可用于保存mesh的信息

**创建ID3DXBuffer接口的对象**
```
HRESULT  D3DXCreateBuffer
(
    DWORD NumBytes,	// 缓存大小
    LPD3DXBUFFER *ppBuffer	 // 返回指针
); 
```

#### D3DXMESH
[MSDN官方定义](https://docs.microsoft.com/en-us/windows/win32/direct3d9/d3dxmesh)

## D3DXCreateTextureFromFile
> 从文件创建纹理

```
D3DXCreateTextureFromFile(g_pd3dDevice, filename,&g_pMeshTextures[i])
```

### 函数原型
```
HRESULT D3DXCreateTextureFromFile(
  _In_  LPDIRECT3DDEVICE9  pDevice,     \\ 指向IDirect3DDevice9接口的指针，该接口表示要与纹理关联的设备
  _In_  LPCTSTR            pSrcFile,    \\ 指向指定文件名的字符串的指针
  _Out_ LPDIRECT3DTEXTURE9 *ppTexture   \\ 该指针表示创建的纹理对象
);
```

## RemovePathFromFileName

```
void RemovePathFromFileName(LPSTR fullPath, LPWSTR fileName)
{
	//先将fullPath的类型变换为LPWSTR
	WCHAR wszBuf[MAX_PATH];
	MultiByteToWideChar(CP_ACP, 0, fullPath, -1, wszBuf, MAX_PATH);
	wszBuf[MAX_PATH - 1] = L'\0';
	WCHAR* wszFullPath = wszBuf;

	//从绝对路径中提取文件名
	LPWSTR pch = wcsrchr(wszFullPath, '\\');
	if (pch)
	lstrcpy(fileName, ++pch);
	else
	lstrcpy(fileName, wszFullPath);
}
```

### MultiByteToWideChar
> 将字符串映射到UTF-16（宽字符）字符串

[MSDN官方定义](https://docs.microsoft.com/en-us/windows/win32/api/stringapiset/nf-stringapiset-multibytetowidechar)

### lstrcpyA
> 将字符串复制到缓冲区

```
LPSTR lstrcpyA(
  LPSTR  lpString1, \\ 目标缓冲区
  LPCSTR lpString2  \\ 源缓冲区
);
```

# Render部分增加
```
//逐子集渲染网格模型
for (DWORD i = 0; i < g_dwNumMaterials; i++)
{
    //设置材料和纹理
    g_pd3dDevice->SetMaterial(&g_pMeshMaterials[i]);
    g_pd3dDevice->SetTexture(0, g_pMeshTextures[i]);

    //渲染模型
    g_pMesh->DrawSubset(i);
}
```

# Cleanup部分增加
```
//释放网格模型材质
SAFE_DELETE_ARRAY(g_pMeshMaterials);

//释放网格模型纹理
if (g_pMeshTextures)
{
    for (DWORD i = 0; i < g_dwNumMaterials; i++)
    {
        SAFE_RELEASE(g_pMeshTextures[i]);
    }

    delete[] g_pMeshTextures;
}
SAFE_RELEASE(g_pMesh);
```