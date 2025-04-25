我的工程结构为（项目根目录是talkwalk）
talkwalk/（根目录）
            manage.py
            db.sqlite3
	    talkwalk/（一级目录）
                         _init_.py
			 asgi.py
			 settings.py
			 urls.py
		         wsgi.py	

            account/（一级目录）
			_init_.py
			admin.py
			apps.py
			forms.py
			models.py
			tests.py
			urls.py
			views.py

	    agency/（一级目录）
		       _init_.py
		       admin.py
		       apps.py
		       models.py
		       tests.py
		       urls.py
		       views.py

	    routes/（一级目录）
		      _init_.py
		      admin.py
		      apps.py
		      models.py
		      tests.py
		      urls.py
		      views.py

            templates/（一级目录）
                            base.html
                           accounts/（二级目录）
                                         login.html
  					 profile.html
                                         register.html
                                         register_success.html

                           agency/（二级目录） 
                                      analytics.html
                                      booking_management.html
                                      dashboard.html
                                      guide_list.html
				      itinerary_list.html
                                      notifications.html
                                      route_edit.html
                                      route_management.html
                                      schedule_management.html
                                      tourism_policies.html

                            routes/（二级目录）
                                      route_detail.html
				      route_form.html
                                      route_list.html

下面是每个文件的内容
① talkwalk/ talkwalk/
（1）talkwalk/ talkwalk/ asgi.py：
"""
ASGI config for My_New_OTA project.

It exposes the ASGI callable as a module-level variable named ``application``.

For more information on this file, see
https://docs.djangoproject.com/en/5.2/howto/deployment/asgi/
"""

import os

from django.core.asgi import get_asgi_application

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'My_New_OTA.settings')

application = get_asgi_application()

（2）talkwalk/ talkwalk/settings.py:
# talkwalk/settings.py
import os
from pathlib import Path

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'django-insecure-your-secret-key-here'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True
AMAP_API_KEY = 'ccbf15b40714d302f5476317b7d20c22'
ALLOWED_HOSTS = []

# Application definition
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 自定义应用
    'accounts',
    'agency',
    'routes',
    # 第三方应用
    'crispy_forms',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'allauth.account.middleware.AccountMiddleware',

]

ROOT_URLCONF = 'talkwalk.urls'

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'talkwalk.wsgi.application'

# Database
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# Password validation
AUTH_PASSWORD_VALIDATORS = [
    # {
    #     'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    # },
    # {
    #     'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    # },
    # {
    #     'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    # },
    # {
    #     'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    # },
]

# 自定义用户模型
AUTH_USER_MODEL = 'accounts.CustomUser'

# 认证设置
AUTHENTICATION_BACKENDS = [
    'django.contrib.auth.backends.ModelBackend',
    'allauth.account.auth_backends.AuthenticationBackend',
]

# Internationalization
LANGUAGE_CODE = 'zh-hans'  # 使用中文
TIME_ZONE = 'Asia/Shanghai'  # 使用中国时区
USE_I18N = True
USE_TZ = True

# Static files (CSS, JavaScript, Images)
STATIC_URL = 'static/'
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]

# 媒体文件设置
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

# Default primary key field type
DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'

# 登录重定向设置
LOGIN_REDIRECT_URL = '/dashboard/'
LOGOUT_REDIRECT_URL = '/'

(3)talkwalk/ talkwalk/urls.py:
# talkwalk/urls.py
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('accounts.urls')),
    path('agency/', include('agency.urls')),
    path('routes/', include('routes.urls')),
]

# 添加媒体文件的URL
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
(4)talkwalk/ talkwalk/wsgi.py:
"""
WSGI config for My_New_OTA project.

It exposes the WSGI callable as a module-level variable named ``application``.

For more information on this file, see
https://docs.djangoproject.com/en/5.2/howto/deployment/wsgi/
"""

import os

from django.core.wsgi import get_wsgi_application

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'My_New_OTA.settings')

application = get_wsgi_application()


②talkwalk/accounts/
（1）talkwalk/accounts/admin.py:
from django.contrib import admin

# Register your models here.

(2)talkwalk/accounts/apps.py:
from django.apps import AppConfig


class AccountsConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'accounts'

(3)talkwalk/accounts/forms.py:
# accounts/forms.py
from django import forms
from django.contrib.auth.forms import UserCreationForm, AuthenticationForm
from .models import CustomUser, TravelAgency, Guide, Tourist


class CustomUserCreationForm(UserCreationForm):
    user_type = forms.ChoiceField(
        choices=CustomUser.USER_TYPE_CHOICES[:-1],  # 排除主管部门选项
        label='用户类型',
        widget=forms.Select(attrs={'class': 'form-control'})
    )

    phone = forms.CharField(
        label='手机号码',
        max_length=20,
        widget=forms.TextInput(attrs={'class': 'form-control'}),
        required=True
    )

    # 动态字段，根据用户类型不同显示不同名称
    name = forms.CharField(
        label='姓名/旅行社名称',
        max_length=100,
        widget=forms.TextInput(attrs={'class': 'form-control'}),
        required=True
    )

    class Meta:
        model = CustomUser
        fields = ('username', 'email', 'phone', 'name', 'user_type', 'password1', 'password2')
        widgets = {
            'username': forms.TextInput(attrs={'class': 'form-control'}),
            'email': forms.EmailInput(attrs={'class': 'form-control'}),
        }

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.fields['password1'].widget = forms.PasswordInput(attrs={'class': 'form-control'})
        self.fields['password2'].widget = forms.PasswordInput(attrs={'class': 'form-control'})


class CustomAuthenticationForm(AuthenticationForm):
    username = forms.CharField(widget=forms.TextInput(attrs={'class': 'form-control'}))
    password = forms.CharField(widget=forms.PasswordInput(attrs={'class': 'form-control'}))


class TravelAgencyForm(forms.ModelForm):
    class Meta:
        model = TravelAgency
        fields = ('agency_name', 'license_number', 'address', 'description', 'established_date')
        widgets = {
            'agency_name': forms.TextInput(attrs={'class': 'form-control'}),
            'license_number': forms.TextInput(attrs={'class': 'form-control'}),
            'address': forms.TextInput(attrs={'class': 'form-control'}),
            'description': forms.Textarea(attrs={'class': 'form-control'}),
            'established_date': forms.DateInput(attrs={'class': 'form-control', 'type': 'date'}),
        }


class GuideForm(forms.ModelForm):
    class Meta:
        model = Guide
        fields = ('license_number', 'experience_years', 'specialty')
        widgets = {
            'license_number': forms.TextInput(attrs={'class': 'form-control'}),
            'experience_years': forms.NumberInput(attrs={'class': 'form-control'}),
            'specialty': forms.TextInput(attrs={'class': 'form-control'}),
        }


class TouristForm(forms.ModelForm):
    class Meta:
        model = Tourist
        fields = ('id_number', 'birth_date')
        widgets = {
            'id_number': forms.TextInput(attrs={'class': 'form-control'}),
            'birth_date': forms.DateInput(attrs={'class': 'form-control', 'type': 'date'}),
        }


class ProfileUpdateForm(forms.ModelForm):
    class Meta:
        model = CustomUser
        fields = ('username', 'email', 'profile_pic', 'phone')
        widgets = {
            'username': forms.TextInput(attrs={'class': 'form-control'}),
            'email': forms.EmailInput(attrs={'class': 'form-control'}),
            'phone': forms.TextInput(attrs={'class': 'form-control'}),
        }
(4)talkwalk/accounts/models.py:# accounts/models.py
from django.db import models
from django.contrib.auth.models import AbstractUser
from django.utils.translation import gettext_lazy as _


class CustomUser(AbstractUser):
    USER_TYPE_CHOICES = (
        ('agency', '旅行社'),
        ('guide', '导游'),
        ('tourist', '游客'),
        ('admin_dept', '主管部门'),
    )

    user_type = models.CharField(
        _('用户类型'),
        max_length=10,
        choices=USER_TYPE_CHOICES,
        default='tourist',
    )

    profile_pic = models.ImageField(
        _('头像'),
        upload_to='profile_pics/',
        null=True,
        blank=True
    )

    phone = models.CharField(_('手机号'), max_length=20, blank=True)

    def __str__(self):
        return self.username


class TravelAgency(models.Model):
    user = models.OneToOneField(
        CustomUser,
        on_delete=models.CASCADE,
        related_name='travel_agency'
    )
    agency_name = models.CharField(_('旅行社名称'), max_length=100)
    license_number = models.CharField(_('营业执照号'), max_length=50, blank=True)
    address = models.CharField(_('地址'), max_length=200, blank=True)
    description = models.TextField(_('简介'), blank=True)
    established_date = models.DateField(_('成立日期'), null=True, blank=True)

    def __str__(self):
        return self.agency_name


class Guide(models.Model):
    user = models.OneToOneField(
        CustomUser,
        on_delete=models.CASCADE,
        related_name='guide'
    )
    agency = models.ForeignKey(
        TravelAgency,
        on_delete=models.SET_NULL,
        null=True,
        blank=True,
        related_name='guides'
    )
    license_number = models.CharField(_('导游证号'), max_length=50)
    experience_years = models.PositiveIntegerField(_('从业年限'), default=0)
    specialty = models.CharField(_('专长'), max_length=100, blank=True)
    rating = models.FloatField(_('评分'), default=5.0)
    salary = models.DecimalField(_('工资'), max_digits=10, decimal_places=2, default=0)
    complaint_count = models.PositiveIntegerField(_('投诉次数'), default=0)

    def __str__(self):
        return f"{self.user.username} - {self.license_number}"


class Tourist(models.Model):
    user = models.OneToOneField(
        CustomUser,
        on_delete=models.CASCADE,
        related_name='tourist'
    )
    id_number = models.CharField(_('身份证号'), max_length=18, blank=True)
    birth_date = models.DateField(_('出生日期'), null=True, blank=True)

    def __str__(self):
        return self.user.username
(6)talkwalk/accounts/tests.py:
from django.test import TestCase

# Create your tests here.

(7)talkwalk/accounts/urls.py:
# accounts/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('register/', views.register_view, name='register'),
    path('login/', views.login_view, name='login'),
    path('logout/', views.logout_view, name='logout'),
    path('profile/', views.profile_view, name='profile'),
    path('update-profile-pic/', views.update_profile_pic, name='update_profile_pic'),
    path('clear-users/', views.clear_users, name='clear_users'),  # 添加清除用户功能
    path('', views.login_view, name='home'),  # 将根URL映射到登录页面
]
(8)talkwalk/accounts/views.py:# accounts/views.py
# accounts/views.py
from django.shortcuts import render, redirect, get_object_or_404
from django.contrib.auth import login, authenticate, logout
from django.contrib.auth.decorators import login_required
from django.contrib import messages
from .forms import (
    CustomUserCreationForm, CustomAuthenticationForm,
    TravelAgencyForm, GuideForm, TouristForm, ProfileUpdateForm
)
from .models import CustomUser, TravelAgency, Guide, Tourist
from django.http import JsonResponse
from django.db import transaction


def register_view(request):
    if request.method == 'POST':
        form = CustomUserCreationForm(request.POST)
        phone = request.POST.get('phone', '')
        name = request.POST.get('name', '')
        user_type = request.POST.get('user_type', '')

        # 检查必填字段
        if not phone or not name or not user_type:
            if not phone:
                form.add_error(None, '电话号码是必填项')
            if not name:
                form.add_error(None, '姓名/旅行社名称是必填项')
            if not user_type:
                form.add_error(None, '用户类型是必填项')

            # 返回表单和错误信息
            return render(request, 'accounts/register.html', {'form': form})

        if form.is_valid():
            try:
                with transaction.atomic():
                    user = form.save(commit=False)
                    user.user_type = user_type
                    user.phone = phone
                    user.save()

                    # 根据用户类型创建相应的附加信息
                    if user_type == 'agency':
                        TravelAgency.objects.create(user=user, agency_name=name)
                    elif user_type == 'guide':
                        Guide.objects.create(user=user, license_number='待填写')
                    elif user_type == 'tourist':
                        Tourist.objects.create(user=user)

                    # 注册成功，返回带倒计时的登录页面
                    return render(request, 'accounts/register_success.html', {
                        'username': user.username,
                        'countdown': 5  # 5秒倒计时
                    })
            except Exception as e:
                form.add_error(None, f"注册过程中发生错误: {str(e)}")
    else:
        form = CustomUserCreationForm()

    return render(request, 'accounts/register.html', {'form': form})


def login_view(request):
    if request.method == 'POST':
        form = CustomAuthenticationForm(request, data=request.POST)
        if form.is_valid():
            username = form.cleaned_data.get('username')
            password = form.cleaned_data.get('password')
            user = authenticate(username=username, password=password)
            if user is not None:
                login(request, user)
                # 根据用户类型重定向到不同的仪表板
                if user.user_type == 'agency':
                    return redirect('agency:dashboard')
                elif user.user_type == 'guide':
                    return redirect('guide_dashboard')  # 这个URL需要在后续实现
                elif user.user_type == 'tourist':
                    return redirect('tourist_dashboard')  # 这个URL需要在后续实现
                else:
                    return redirect('admin_dashboard')  # 这个URL需要在后续实现
            else:
                messages.error(request, "用户名或密码错误。")
        else:
            messages.error(request, "用户名或密码错误。")
    else:
        form = CustomAuthenticationForm()

    return render(request, 'accounts/login.html', {'form': form})


# 添加清除用户功能（仅用于开发测试）
def clear_users(request):
    if request.method == 'POST' and request.user.is_superuser:
        # 仅超级用户可以执行此操作
        # 保留超级用户账号
        CustomUser.objects.exclude(is_superuser=True).delete()
        messages.success(request, "已清除所有非管理员用户")
    else:
        messages.error(request, "权限不足")

    return redirect('login')

def logout_view(request):
    logout(request)
    return redirect('login')


@login_required
def profile_view(request):
    if request.method == 'POST':
        form = ProfileUpdateForm(request.POST, request.FILES, instance=request.user)
        if form.is_valid():
            form.save()

            # 如果是旅行社，还需要更新旅行社名称
            if request.user.user_type == 'agency':
                agency_name = request.POST.get('agency_name')
                if agency_name:
                    agency = TravelAgency.objects.get(user=request.user)
                    agency.agency_name = agency_name
                    agency.save()

            messages.success(request, '个人信息已更新！')
            return redirect('profile')
    else:
        form = ProfileUpdateForm(instance=request.user)

    context = {
        'form': form,
        'user': request.user
    }

    # 根据用户类型添加额外的表单
    if request.user.user_type == 'agency':
        agency = get_object_or_404(TravelAgency, user=request.user)
        context['agency'] = agency

    return render(request, 'accounts/profile.html', context)


@login_required
def update_profile_pic(request):
    if request.method == 'POST' and request.FILES.get('profile_pic'):
        user = request.user
        user.profile_pic = request.FILES.get('profile_pic')
        user.save()
        return JsonResponse({'success': True, 'image_url': user.profile_pic.url})
    return JsonResponse({'success': False})

③talkwalk/agency/:
(1)talkwalk/agency/admin.py::
from django.contrib import admin

# Register your models here.

(2)talkwalk/agency/apps.py:
from django.apps import AppConfig


class AgencyConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'agency'

(3)talkwalk/agency/models.py:
from django.db import models
from accounts.models import CustomUser, Guide, TravelAgency
from django.utils import timezone


# 行程模型
class Itinerary(models.Model):
    """行程模型：一个行程可以包含多条路线"""
    agency = models.ForeignKey(TravelAgency, on_delete=models.CASCADE, related_name='itineraries')
    name = models.CharField(max_length=100)
    description = models.TextField(blank=True)
    price = models.DecimalField(max_digits=10, decimal_places=2, default=0)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.name


# 路线模型
class Route(models.Model):
    """路线模型：属于某个行程，包含多个景点"""
    itinerary = models.ForeignKey(Itinerary, on_delete=models.CASCADE, related_name='routes')
    name = models.CharField(max_length=100)
    description = models.TextField(blank=True)
    order = models.IntegerField(default=0)  # 在行程中的顺序
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        ordering = ['itinerary', 'order']

    def __str__(self):
        return f"{self.itinerary.name} - {self.name}"


# 景点模型
class Spot(models.Model):
    """景点模型：属于某条路线"""
    route = models.ForeignKey(Route, on_delete=models.CASCADE, related_name='spots')
    name = models.CharField(max_length=100)
    description = models.TextField(blank=True)
    latitude = models.FloatField()
    longitude = models.FloatField()
    order = models.IntegerField(default=0)  # 在路线中的顺序

    class Meta:
        ordering = ['route', 'order']

    def __str__(self):
        return self.name


# 景点图片模型
class SpotImage(models.Model):
    """景点图片"""
    spot = models.ForeignKey(Spot, on_delete=models.CASCADE, related_name='images')
    image = models.ImageField(upload_to='spots/')
    caption = models.CharField(max_length=200, blank=True)
    order = models.IntegerField(default=0)

    class Meta:
        ordering = ['order']


# 景点连接模型
class SpotConnection(models.Model):
    """景点连接模型：定义两个景点之间的连接路径"""
    route = models.ForeignKey(Route, on_delete=models.CASCADE, related_name='connections')
    from_spot = models.ForeignKey(Spot, on_delete=models.CASCADE, related_name='outgoing_connections')
    to_spot = models.ForeignKey(Spot, on_delete=models.CASCADE, related_name='incoming_connections')
    path_type = models.CharField(max_length=50, choices=[
        ('walking', '步行'),
        ('driving', '驾车'),
        ('transit', '公交'),
        ('custom', '自定义'),
    ], default='driving')
    path_coords = models.TextField(blank=True, help_text='存储路径坐标的JSON字符串')

    class Meta:
        unique_together = ('route', 'from_spot', 'to_spot')


# 行程安排模型
class Schedule(models.Model):
    """行程安排：某条路线的具体执行计划"""
    route = models.ForeignKey(Route, on_delete=models.CASCADE, related_name='schedules')
    guide = models.ForeignKey(Guide, on_delete=models.SET_NULL, null=True, blank=True, related_name='schedules')
    title = models.CharField(max_length=100)
    description = models.TextField(blank=True)
    start_date = models.DateTimeField()
    end_date = models.DateTimeField()
    max_tourists = models.IntegerField(default=20)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    status = models.CharField(max_length=20, choices=[
        ('planned', '计划中'),
        ('ongoing', '进行中'),
        ('completed', '已完成'),
        ('cancelled', '已取消'),
    ], default='planned')
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f"{self.route.name} - {self.start_date.strftime('%Y-%m-%d')}"

    @property
    def current_tourists(self):
        return self.bookings.count()


# 预订模型
class Booking(models.Model):
    """游客预订"""
    schedule = models.ForeignKey(Schedule, on_delete=models.CASCADE, related_name='bookings')
    tourist = models.ForeignKey('accounts.Tourist', on_delete=models.CASCADE, related_name='bookings')
    booking_date = models.DateTimeField(auto_now_add=True)
    tourist_count = models.IntegerField(default=1)
    total_price = models.DecimalField(max_digits=10, decimal_places=2, default=0)
    status = models.CharField(max_length=20, choices=[
        ('pending', '待付款'),
        ('paid', '已付款'),
        ('completed', '已完成'),
        ('cancelled', '已取消'),
        ('refunded', '已退款')
    ], default='pending')

    def __str__(self):
        return f"{self.tourist.user.username} - {self.schedule}"


# 导游邀请模型
class GuideInvitation(models.Model):
    agency = models.ForeignKey(TravelAgency, on_delete=models.CASCADE, related_name='sent_invitations')
    guide = models.ForeignKey(Guide, on_delete=models.CASCADE, related_name='received_invitations')
    message = models.TextField(blank=True)
    status = models.CharField(max_length=20, choices=[
        ('pending', '待处理'),
        ('accepted', '已接受'),
        ('rejected', '已拒绝')
    ], default='pending')
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        ordering = ['-created_at']
        verbose_name = '导游邀请'
        verbose_name_plural = '导游邀请'


# 通知模型
class Notification(models.Model):
    sender = models.ForeignKey(CustomUser, on_delete=models.CASCADE, related_name='sent_notifications')
    receiver = models.ForeignKey(CustomUser, on_delete=models.CASCADE, related_name='received_notifications')
    message = models.TextField()
    is_read = models.BooleanField(default=False)
    notification_type = models.CharField(max_length=50, default='message')
    related_id = models.IntegerField(null=True, blank=True)  # 用于存储与通知相关的对象ID
    created_at = models.DateTimeField(auto_now_add=True)

    class Meta:
        ordering = ['-created_at']
        verbose_name = '通知'
        verbose_name_plural = '通知'


# 旅游政策模型
class TourismPolicy(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    publisher = models.CharField(max_length=100)
    publish_date = models.DateTimeField(default=timezone.now)
    attachment = models.FileField(upload_to='policies/', null=True, blank=True)

    class Meta:
        ordering = ['-publish_date']
        verbose_name = '旅游政策'
        verbose_name_plural = '旅游政策'
(4)talkwalk/agency/test.py:
from django.test import TestCase

# Create your tests here.

(5)talkwalk/agency/urls.py:
from django.urls import path
from . import views

app_name = 'agency'

urlpatterns = [
    path('dashboard/', views.dashboard, name='dashboard'),
    path('guides/', views.guide_list, name='guide_list'),
    path('guides/available/', views.get_available_guides, name='get_available_guides'),
    path('guides/invite/', views.invite_guide, name='invite_guide'),
    path('routes/', views.route_management, name='route_management'),
    path('routes/editor/', views.route_editor, name='route_editor_new'),
    path('routes/editor/<int:route_id>/', views.route_editor, name='route_editor'),
    path('routes/save/', views.save_route, name='save_route'),
    path('bookings/', views.booking_management, name='booking_management'),
    path('schedules/', views.schedule_management, name='schedule_management'),
    path('notifications/', views.notifications, name='notifications'),
    path('notifications/send/', views.send_notification, name='send_notification'),
    path('policies/', views.tourism_policies, name='tourism_policies'),
    path('analytics/', views.analytics, name='analytics'),

    # 新增行程管理相关URL
    path('itineraries/', views.itinerary_list, name='itinerary_list'),
    path('itineraries/create/', views.itinerary_create, name='itinerary_create'),
    path('itineraries/<int:itinerary_id>/', views.itinerary_detail, name='itinerary_detail'),
    path('itineraries/<int:itinerary_id>/routes/create/', views.route_create, name='route_create'),
    path('routes/<int:route_id>/edit/', views.route_edit, name='route_edit'),
    path('routes/<int:route_id>/spots/create/', views.spot_create, name='spot_create'),
    path('spots/<int:spot_id>/edit/', views.spot_edit, name='spot_edit'),
    path('spots/<int:spot_id>/delete/', views.spot_delete, name='spot_delete'),
    path('connections/<int:connection_id>/update/', views.connection_update, name='connection_update'),
]

(6)talkwalk/agency/views.py:from django.shortcuts import render, redirect, get_object_or_404
from django.contrib.auth.decorators import login_required
from accounts.models import CustomUser, Guide, TravelAgency, Tourist
from django.http import JsonResponse
from .decorators import agency_required
from .models import Route, Spot, SpotImage, Schedule, Booking, Notification, GuideInvitation, Itinerary, SpotConnection, \
    TourismPolicy
from django.db.models import Sum, Count, Q
import json
from django.utils import timezone
from django.contrib import messages
from django.conf import settings


# # 装饰器函数，确保只有旅行社可以访问
# def agency_required(view_func):
#     def wrapper(request, *args, **kwargs):
#         if request.user.user_type != 'agency':
#             return redirect('login')
#         return view_func(request, *args, **kwargs)
#
#     return wrapper


@login_required
@agency_required
def dashboard(request):
    """旅行社仪表板"""
    agency = TravelAgency.objects.get(user=request.user)

    # 统计数据
    guide_count = Guide.objects.filter(agency=agency).count()
    booking_count = Booking.objects.filter(schedule__route__itinerary__agency=agency).count()
    total_revenue = Booking.objects.filter(
        schedule__route__itinerary__agency=agency,
        status='completed'
    ).aggregate(total=Sum('total_price'))['total'] or 0
    route_count = Route.objects.filter(itinerary__agency=agency).count()

    # 获取近期行程
    upcoming_schedules = Schedule.objects.filter(
        route__itinerary__agency=agency,
        start_date__gte=timezone.now()
    ).order_by('start_date')[:5]

    context = {
        'agency': agency,
        'guide_count': guide_count,
        'booking_count': booking_count,
        'total_revenue': total_revenue,
        'route_count': route_count,
        'upcoming_schedules': upcoming_schedules
    }

    return render(request, 'agency/dashboard.html', context)


@login_required
@agency_required
def guide_list(request):
    """导游列表"""
    agency = TravelAgency.objects.get(user=request.user)
    guides = Guide.objects.filter(agency=agency)

    # 导游申请
    guide_applications = GuideInvitation.objects.filter(agency=agency)

    context = {
        'agency': agency,
        'guides': guides,
        'guide_applications': guide_applications
    }

    return render(request, 'agency/guide_list.html', context)


@login_required
@agency_required
def get_available_guides(request):
    """获取没有旅行社归属的导游列表"""
    # 获取没有旅行社的导游
    available_guides = Guide.objects.filter(agency__isnull=True)

    return JsonResponse({
        'success': True,
        'guides': list(
            available_guides.values('id', 'user__username', 'user__id', 'license_number', 'experience_years'))
    })


@login_required
@agency_required
def invite_guide(request):
    """邀请导游加入旅行社"""
    if request.method == 'POST':
        guide_id = request.POST.get('guide_id')
        message = request.POST.get('message')

        if not guide_id:
            return JsonResponse({'success': False, 'error': '请选择导游'})

        try:
            agency = TravelAgency.objects.get(user=request.user)
            guide = Guide.objects.get(id=guide_id)

            # 检查导游是否已有旅行社
            if guide.agency:
                return JsonResponse({'success': False, 'error': '该导游已属于其他旅行社'})

            # 创建邀请
            invitation = GuideInvitation.objects.create(
                agency=agency,
                guide=guide,
                message=message,
                status='pending'
            )

            # 创建通知
            Notification.objects.create(
                sender=request.user,
                receiver=guide.user,
                message=f"{agency.agency_name}邀请您加入他们的旅行社。",
                notification_type='guide_invitation',
                related_id=invitation.id
            )

            return JsonResponse({'success': True})
        except Guide.DoesNotExist:
            return JsonResponse({'success': False, 'error': '导游不存在'})
        except Exception as e:
            return JsonResponse({'success': False, 'error': str(e)})

    return JsonResponse({'success': False, 'error': '非法请求'})


@login_required
@agency_required
def route_management(request):
    """路线管理"""
    agency = TravelAgency.objects.get(user=request.user)
    routes = Route.objects.filter(itinerary__agency=agency).select_related('itinerary')

    context = {
        'agency': agency,
        'routes': routes
    }

    return render(request, 'agency/route_management.html', context)


@login_required
@agency_required
def route_editor(request, route_id=None):
    """路线编辑器"""
    agency = TravelAgency.objects.get(user=request.user)

    if route_id:
        route = get_object_or_404(Route, id=route_id, itinerary__agency=agency)
        spots = Spot.objects.filter(route=route).order_by('order')
    else:
        route = None
        spots = []

    context = {
        'agency': agency,
        'route': route,
        'spots': spots,
        'api_key': settings.AMAP_API_KEY  # 确保在settings.py中设置了AMAP_API_KEY
    }

    return render(request, 'agency/route_editor.html', context)


@login_required
@agency_required
def save_route(request):
    """保存路线"""
    if request.method == 'POST':
        try:
            data = json.loads(request.body)
            route_id = data.get('route_id')
            route_name = data.get('route_name')
            route_description = data.get('route_description')

            agency = TravelAgency.objects.get(user=request.user)

            if route_id:
                route = get_object_or_404(Route, id=route_id, itinerary__agency=agency)
                route.name = route_name
                route.description = route_description
                route.save()
            else:
                # 如果是新路线，需要先创建一个行程
                itinerary = Itinerary.objects.create(
                    agency=agency,
                    name=f"{route_name} 行程",
                    description=route_description
                )
                route = Route.objects.create(
                    itinerary=itinerary,
                    name=route_name,
                    description=route_description
                )

            return JsonResponse({
                'success': True,
                'route_id': route.id
            })
        except Exception as e:
            return JsonResponse({
                'success': False,
                'error': str(e)
            })

    return JsonResponse({'success': False, 'error': '非法请求'})


@login_required
@agency_required
def booking_management(request):
    """预订管理"""
    agency = TravelAgency.objects.get(user=request.user)
    bookings = Booking.objects.filter(schedule__route__itinerary__agency=agency)

    context = {
        'agency': agency,
        'bookings': bookings
    }

    return render(request, 'agency/booking_management.html', context)


@login_required
@agency_required
def schedule_management(request):
    """行程安排"""
    agency = TravelAgency.objects.get(user=request.user)
    schedules = Schedule.objects.filter(route__itinerary__agency=agency)

    context = {
        'agency': agency,
        'schedules': schedules
    }

    return render(request, 'agency/schedule_management.html', context)


@login_required
@agency_required
def notifications(request):
    """通知页面"""
    agency = TravelAgency.objects.get(user=request.user)

    context = {
        'agency': agency
    }

    return render(request, 'agency/notifications.html', context)


@login_required
@agency_required
def send_notification(request):
    """发送通知/消息"""
    if request.method == 'POST':
        receiver_id = request.POST.get('receiver_id')
        message_content = request.POST.get('message')

        if not receiver_id or not message_content:
            return JsonResponse({'success': False, 'error': '接收者和消息内容不能为空'})

        try:
            receiver = CustomUser.objects.get(id=receiver_id)

            # 创建通知
            notification = Notification.objects.create(
                sender=request.user,
                receiver=receiver,
                message=message_content,
                notification_type='message'
            )

            return JsonResponse({
                'success': True,
                'notification_id': notification.id,
                'created_at': notification.created_at.strftime('%Y-%m-%d %H:%M')
            })
        except CustomUser.DoesNotExist:
            return JsonResponse({'success': False, 'error': '接收者不存在'})
        except Exception as e:
            return JsonResponse({'success': False, 'error': str(e)})

    return JsonResponse({'success': False, 'error': '无效的请求方法'})


@login_required
@agency_required
def analytics(request):
    """统计分析"""
    agency = TravelAgency.objects.get(user=request.user)

    context = {
        'agency': agency
    }

    return render(request, 'agency/analytics.html', context)


@login_required
def tourism_policies(request):
    """显示旅游政策列表"""
    policies = TourismPolicy.objects.all()

    context = {
        'policies': policies
    }

    # 如果是旅行社用户，添加旅行社信息
    if request.user.user_type == 'agency':
        context['agency'] = TravelAgency.objects.get(user=request.user)

    return render(request, 'agency/tourism_policies.html', context)


@login_required
@agency_required
def itinerary_list(request):
    """行程列表"""
    agency = get_object_or_404(TravelAgency, user=request.user)
    itineraries = Itinerary.objects.filter(agency=agency)

    context = {
        'agency': agency,
        'itineraries': itineraries
    }

    return render(request, 'agency/itinerary_list.html', context)


@login_required
@agency_required
def itinerary_create(request):
    """创建行程"""
    agency = get_object_or_404(TravelAgency, user=request.user)

    if request.method == 'POST':
        name = request.POST.get('name')
        description = request.POST.get('description', '')
        price = request.POST.get('price', 0)

        if not name:
            messages.error(request, '行程名称不能为空')
            return redirect('agency:itinerary_create')

        itinerary = Itinerary.objects.create(
            agency=agency,
            name=name,
            description=description,
            price=price
        )

        messages.success(request, f'行程 "{name}" 创建成功')
        return redirect('agency:itinerary_detail', itinerary_id=itinerary.id)

    context = {
        'agency': agency
    }

    return render(request, 'agency/itinerary_form.html', context)


@login_required
@agency_required
def itinerary_detail(request, itinerary_id):
    """行程详情"""
    agency = get_object_or_404(TravelAgency, user=request.user)
    itinerary = get_object_or_404(Itinerary, id=itinerary_id, agency=agency)
    routes = Route.objects.filter(itinerary=itinerary).prefetch_related('spots')

    context = {
        'agency': agency,
        'itinerary': itinerary,
        'routes': routes
    }

    return render(request, 'agency/itinerary_detail.html', context)


@login_required
@agency_required
def route_create(request, itinerary_id):
    """创建路线"""
    agency = get_object_or_404(TravelAgency, user=request.user)
    itinerary = get_object_or_404(Itinerary, id=itinerary_id, agency=agency)

    if request.method == 'POST':
        name = request.POST.get('name')
        description = request.POST.get('description', '')

        if not name:
            messages.error(request, '路线名称不能为空')
            return redirect('agency:route_create', itinerary_id=itinerary.id)

        # 计算顺序
        order = Route.objects.filter(itinerary=itinerary).count() + 1

        route = Route.objects.create(
            itinerary=itinerary,
            name=name,
            description=description,
            order=order
        )

        messages.success(request, f'路线 "{name}" 创建成功')
        return redirect('agency:route_edit', route_id=route.id)

    context = {
        'agency': agency,
        'itinerary': itinerary
    }

    return render(request, 'agency/route_form.html', context)


@login_required
@agency_required
def route_edit(request, route_id):
    """编辑路线"""
    agency = get_object_or_404(TravelAgency, user=request.user)
    route = get_object_or_404(Route, id=route_id, itinerary__agency=agency)
    spots = Spot.objects.filter(route=route).order_by('order')
    connections = SpotConnection.objects.filter(route=route)

    if request.method == 'POST':
        route.name = request.POST.get('name', route.name)
        route.description = request.POST.get('description', route.description)
        route.save()
        messages.success(request, '路线信息已更新')

    # 准备连接数据用于前端展示
    connection_data = []
    for conn in connections:
        connection_data.append({
            'id': conn.id,
            'from_spot_id': conn.from_spot_id,
            'to_spot_id': conn.to_spot_id,
            'path_type': conn.path_type,
            'path_coords': conn.path_coords
        })

    context = {
        'agency': agency,
        'route': route,
        'itinerary': route.itinerary,
        'spots': spots,
        'connections': json.dumps(connection_data),
        'api_key': settings.AMAP_API_KEY  # 确保在settings.py中设置了AMAP_API_KEY
    }

    return render(request, 'agency/route_edit.html', context)


@login_required
@agency_required
def spot_create(request, route_id):
    """创建景点"""
    if request.method == 'POST':
        route = get_object_or_404(Route, id=route_id, itinerary__agency__user=request.user)
        data = json.loads(request.body) if request.body else request.POST

        name = data.get('name')
        description = data.get('description', '')
        latitude = data.get('latitude')
        longitude = data.get('longitude')

        if not name or not latitude or not longitude:
            return JsonResponse({'success': False, 'error': '缺少必要字段'})

        # 计算顺序
        order = Spot.objects.filter(route=route).count() + 1

        spot = Spot.objects.create(
            route=route,
            name=name,
            description=description,
            latitude=float(latitude),
            longitude=float(longitude),
            order=order
        )

        # 如果有前一个点，创建连接
        if order > 1:
            previous_spot = Spot.objects.filter(route=route, order=order - 1).first()
            if previous_spot:
                SpotConnection.objects.create(
                    route=route,
                    from_spot=previous_spot,
                    to_spot=spot,
                    path_type='driving'
                )

        return JsonResponse({
            'success': True,
            'spot_id': spot.id,
            'name': spot.name,
            'order': spot.order
        })

    return JsonResponse({'success': False, 'error': '无效的请求方法'})


@login_required
@agency_required
def spot_edit(request, spot_id):
    """编辑景点"""
    spot = get_object_or_404(Spot, id=spot_id, route__itinerary__agency__user=request.user)

    if request.method == 'POST':
        data = json.loads(request.body) if request.body else request.POST

        spot.name = data.get('name', spot.name)
        spot.description = data.get('description', spot.description)
        spot.latitude = float(data.get('latitude', spot.latitude))
        spot.longitude = float(data.get('longitude', spot.longitude))
        spot.save()

        return JsonResponse({
            'success': True,
            'spot_id': spot.id,
            'name': spot.name,
            'latitude': spot.latitude,
            'longitude': spot.longitude
        })

    return JsonResponse({'success': False, 'error': '无效的请求方法'})


@login_required
@agency_required
def spot_delete(request, spot_id):
    """删除景点"""
    spot = get_object_or_404(Spot, id=spot_id, route__itinerary__agency__user=request.user)

    if request.method == 'POST':
        route = spot.route
        order = spot.order

        # 删除与该景点相关的连接
        SpotConnection.objects.filter(
            Q(from_spot=spot) | Q(to_spot=spot)
        ).delete()

        # 删除景点
        spot.delete()

        # 重新排序后面的景点
        spots_to_update = Spot.objects.filter(route=route, order__gt=order)
        for s in spots_to_update:
            s.order -= 1
            s.save()

        # 重建连接
        spots = Spot.objects.filter(route=route).order_by('order')
        for i in range(len(spots) - 1):
            SpotConnection.objects.get_or_create(
                route=route,
                from_spot=spots[i],
                to_spot=spots[i + 1],
                defaults={'path_type': 'driving'}
            )

        return JsonResponse({'success': True})

    return JsonResponse({'success': False, 'error': '无效的请求方法'})


@login_required
@agency_required
def connection_update(request, connection_id):
    """更新连接路径"""
    connection = get_object_or_404(SpotConnection, id=connection_id, route__itinerary__agency__user=request.user)

    if request.method == 'POST':
        data = json.loads(request.body) if request.body else request.POST

        connection.path_type = data.get('path_type', connection.path_type)
        connection.path_coords = data.get('path_coords', connection.path_coords)
        connection.save()

        return JsonResponse({'success': True})

    return JsonResponse({'success': False, 'error': '无效的请求方法'})

④talkwalk/routes/
（1）talkwalk/routes/admin.py:
from django.contrib import admin

# Register your models here.

(2)talkwalk/routes/apps.py:
from django.apps import AppConfig


class RoutesConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'routes'

(3)talkwalk/routes/models.py:
from django.db import models

# Create your models here.

(4)talkwalk/routes/tests.py:
from django.test import TestCase

# Create your tests here.

(5)urls.py:
# talkwalk/routes/urls.py
from django.urls import path
from . import views

app_name = 'routes'

urlpatterns = [
    path('', views.route_list, name='route_list'),
    path('<int:route_id>/', views.route_detail, name='route_detail'),
    path('create/', views.route_create, name='route_create'),
    path('<int:route_id>/edit/', views.route_edit, name='route_edit'),
    path('<int:route_id>/spot/create/', views.spot_create, name='spot_create'),
    path('spot/<int:spot_id>/edit/', views.spot_edit, name='spot_edit'),
    path('spot/<int:spot_id>/delete/', views.spot_delete, name='spot_delete'),
    path('spot/reorder/', views.spot_reorder, name='spot_reorder'),
    path('image/<int:image_id>/delete/', views.image_delete, name='image_delete'),
]

(6)talkwalk/routes/views.py:
# talkwalk/routes/views.py
from django.shortcuts import render, redirect, get_object_or_404
from django.contrib.auth.decorators import login_required
from django.http import JsonResponse, HttpResponseForbidden
from agency.models import Route, Spot, SpotImage
from accounts.models import TravelAgency
from django.conf import settings
import json


def agency_required(view_func):
    """装饰器，确保只有旅行社账户可以访问"""

    def wrapper(request, *args, **kwargs):
        if request.user.is_authenticated and request.user.user_type == 'agency':
            return view_func(request, *args, **kwargs)
        return HttpResponseForbidden("您没有权限访问此页面。")

    return wrapper


@login_required
@agency_required
def route_list(request):
    """显示旅行社的所有路线"""
    agency = get_object_or_404(TravelAgency, user=request.user)
    routes = Route.objects.filter(agency=agency)

    context = {
        'routes': routes,
        'agency': agency
    }

    return render(request, 'routes/route_list.html', context)


@login_required
@agency_required
def route_detail(request, route_id):
    """显示路线详情"""
    agency = get_object_or_404(TravelAgency, user=request.user)
    route = get_object_or_404(Route, id=route_id, agency=agency)
    spots = Spot.objects.filter(route=route).order_by('order')

    context = {
        'route': route,
        'spots': spots,
        'agency': agency
    }

    return render(request, 'routes/route_detail.html', context)


# routes/views.py
@login_required
@agency_required
def route_create(request):
    """创建新路线"""
    agency = get_object_or_404(TravelAgency, user=request.user)

    if request.method == 'POST':
        name = request.POST.get('name')
        description = request.POST.get('description', '')
        price = request.POST.get('price', 0)

        route = Route.objects.create(
            agency=agency,
            name=name,
            description=description,
            price=price
        )

        return redirect('routes:route_edit', route_id=route.id)

    context = {
        'agency': agency,
        'api_key': settings.AMAP_API_KEY
    }

    return render(request, 'routes/route_form.html', context)


@login_required
@agency_required
def route_edit(request, route_id):
    """编辑现有路线"""
    agency = get_object_or_404(TravelAgency, user=request.user)
    route = get_object_or_404(Route, id=route_id, agency=agency)

    if request.method == 'POST':
        route.name = request.POST.get('name')
        route.description = request.POST.get('description', '')
        route.price = request.POST.get('price', 0)
        route.save()

        return redirect('routes:route_detail', route_id=route.id)

    context = {
        'route': route,
        'agency': agency,
        'api_key': settings.AMAP_API_KEY
    }

    return render(request, 'routes/route_form.html', context)


@login_required
@agency_required
def spot_create(request, route_id):
    """为路线添加景点"""
    agency = get_object_or_404(TravelAgency, user=request.user)
    route = get_object_or_404(Route, id=route_id, agency=agency)

    if request.method == 'POST':
        name = request.POST.get('name')
        description = request.POST.get('description', '')
        latitude = request.POST.get('latitude')
        longitude = request.POST.get('longitude')
        order = Spot.objects.filter(route=route).count() + 1

        spot = Spot.objects.create(
            route=route,
            name=name,
            description=description,
            latitude=latitude,
            longitude=longitude,
            order=order
        )

        # 处理上传的图片
        for image_file in request.FILES.getlist('images'):
            SpotImage.objects.create(
                spot=spot,
                image=image_file
            )

        return JsonResponse({'success': True, 'spot_id': spot.id})

    return JsonResponse({'success': False}, status=400)


@login_required
@agency_required
def spot_edit(request, spot_id):
    """编辑景点信息"""
    agency = get_object_or_404(TravelAgency, user=request.user)
    spot = get_object_or_404(Spot, id=spot_id, route__agency=agency)

    if request.method == 'POST':
        spot.name = request.POST.get('name')
        spot.description = request.POST.get('description', '')
        spot.latitude = request.POST.get('latitude')
        spot.longitude = request.POST.get('longitude')
        spot.save()

        # 处理上传的新图片
        for image_file in request.FILES.getlist('images'):
            SpotImage.objects.create(
                spot=spot,
                image=image_file
            )

        return JsonResponse({'success': True})

    return JsonResponse({'success': False}, status=400)


@login_required
@agency_required
def spot_delete(request, spot_id):
    """删除景点"""
    agency = get_object_or_404(TravelAgency, user=request.user)
    spot = get_object_or_404(Spot, id=spot_id, route__agency=agency)

    if request.method == 'POST':
        # 删除景点及其所有图片
        spot.delete()

        # 重新排序剩余的景点
        remaining_spots = Spot.objects.filter(route=spot.route).order_by('order')
        for i, remaining_spot in enumerate(remaining_spots):
            remaining_spot.order = i + 1
            remaining_spot.save()

        return JsonResponse({'success': True})

    return JsonResponse({'success': False}, status=400)


@login_required
@agency_required
def spot_reorder(request):
    """调整景点顺序"""
    agency = get_object_or_404(TravelAgency, user=request.user)

    if request.method == 'POST':
        data = json.loads(request.body)
        spot_ids = data.get('spot_ids', [])

        for i, spot_id in enumerate(spot_ids):
            spot = get_object_or_404(Spot, id=spot_id, route__agency=agency)
            spot.order = i + 1
            spot.save()

        return JsonResponse({'success': True})

    return JsonResponse({'success': False}, status=400)


@login_required
@agency_required
def image_delete(request, image_id):
    """删除景点图片"""
    agency = get_object_or_404(TravelAgency, user=request.user)
    image = get_object_or_404(SpotImage, id=image_id, spot__route__agency=agency)

    if request.method == 'POST':
        image.delete()
        return JsonResponse({'success': True})

    return JsonResponse({'success': False}, status=400)

⑤talkwalk/templates
(1)talkwalk/templates/base.html:
<!-- templates/base.html -->
<!DOCTYPE html>
<html lang="zh-Hans">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Talk&Walk 旅行社管理系统{% endblock %}</title>
    <!-- Bootstrap 5 CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Custom CSS -->
    <style>
        :root {
            --primary-color: #3498db;
            --secondary-color: #2c3e50;
            --accent-color: #e74c3c;
            --light-color: #ecf0f1;
            --dark-color: #34495e;
        }

        body {
            font-family: 'Noto Sans SC', sans-serif;
            background-color: #f7f9fc;
        }

        .navbar-brand {
            font-weight: bold;
            color: var(--primary-color) !important;
        }

        .sidebar {
            min-height: calc(100vh - 56px);
            background-color: var(--secondary-color);
            padding-top: 20px;
        }

        .sidebar .nav-link {
            color: rgba(255,255,255,0.8);
            margin-bottom: 5px;
            border-radius: 5px;
            padding: 10px 15px;
        }

        .sidebar .nav-link:hover {
            color: #fff;
            background-color: rgba(255,255,255,0.1);
        }

        .sidebar .nav-link.active {
            color: #fff;
            background-color: var(--primary-color);
        }

        .sidebar .nav-link .fa {
            margin-right: 10px;
        }

        .content {
            padding: 20px;
        }

        .card {
            border: none;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
            border-radius: 10px;
            margin-bottom: 20px;
        }

        .card-header {
            background-color: #fff;
            border-bottom: 1px solid rgba(0,0,0,0.05);
            font-weight: 600;
        }

        .avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            object-fit: cover;
        }

        .avatar-lg {
            width: 120px;
            height: 120px;
        }

        .profile-dropdown {
            min-width: 200px;
        }

        .btn-primary {
            background-color: var(--primary-color);
            border-color: var(--primary-color);
        }

        .btn-primary:hover {
            background-color: #2980b9;
            border-color: #2980b9;
        }

        .stats-card {
            cursor: pointer;
            transition: transform 0.2s;
        }

        .stats-card:hover {
            transform: translateY(-5px);
        }

        /* 登录和注册页面样式 */
        .login-container {
            max-width: 450px;
            margin: 80px auto;
        }

        .auth-card {
            border-radius: 15px;
            box-shadow: 0 5px 20px rgba(0,0,0,0.1);
            overflow: hidden;
        }

        .auth-card .card-header {
            background-color: var(--primary-color);
            color: white;
            text-align: center;
            padding: 20px;
            font-size: 24px;
            font-weight: 600;
        }

        .auth-card .card-body {
            padding: 30px;
        }

        .form-group {
            margin-bottom: 20px;
        }
    </style>
    {% block extra_css %}{% endblock %}
</head>
<body>
    {% block content %}{% endblock %}

    <!-- Bootstrap Bundle with Popper -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    <!-- jQuery -->
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <!-- Custom JS -->
    {% block extra_js %}{% endblock %}
</body>
</html>

(2)talkwalk/templates/accounts/login.html
<!-- templates/accounts/login.html -->
{% extends 'base.html' %}

{% block title %}登录 - Talk&Walk{% endblock %}

{% block content %}
<div class="container login-container">
    <div class="text-center mb-4">
        <h1 class="display-4">Talk&Walk</h1>
        <p class="lead">旅行社管理系统</p>
    </div>

    <div class="card auth-card">
        <div class="card-header">
            用户登录
        </div>
        <div class="card-body">
            {% if messages %}
                {% for message in messages %}
                    <div class="alert alert-danger">
                        {{ message }}
                    </div>
                {% endfor %}
            {% endif %}

            <form method="post">
                {% csrf_token %}
                <div class="form-group">
                    <label for="{{ form.username.id_for_label }}">用户名</label>
                    {{ form.username }}
                </div>
                <div class="form-group">
                    <label for="{{ form.password.id_for_label }}">密码</label>
                    {{ form.password }}
                </div>
                <div class="d-grid gap-2">
                    <button type="submit" class="btn btn-primary btn-lg">登录</button>
                </div>
            </form>

            <div class="text-center mt-4">
                <p>还没有账号？<a href="{% url 'register' %}">立即注册</a></p>
            </div>
        </div>
    </div>
</div>

{% if registered_username %}
<script>
    // 如果有注册成功的用户名，自动填充
    document.addEventListener('DOMContentLoaded', function() {
        document.getElementById('id_username').value = "{{ registered_username }}";
    });
</script>
{% endif %}
{% endblock %}
(3)talkwalk/templates/profile.html
<!-- templates/accounts/profile.html -->
{% extends 'base.html' %}

{% block title %}个人资料 - Talk&Walk{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:schedule_management' %}">
                        <i class="fa fa-calendar"></i> 行程安排
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>

        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h2>个人资料</h2>
            </div>

            <div class="card">
                <div class="card-body">
                    <div class="row">
                        <div class="col-lg-3 text-center">
                            <div class="mb-4">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="Profile Picture" class="avatar-lg" id="profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/120" alt="Default Profile" class="avatar-lg" id="profile-pic">
                                {% endif %}
                            </div>

                            <div class="mb-3">
                                <form id="profile-pic-form" enctype="multipart/form-data">
                                    {% csrf_token %}
                                    <label for="id_profile_pic" class="btn btn-sm btn-outline-primary">
                                        更改头像
                                    </label>
                                    <input type="file" name="profile_pic" id="id_profile_pic" accept="image/*" style="display: none;">
                                </form>
                            </div>
                        </div>

                        <div class="col-lg-9">
                            <form method="post">
                                {% csrf_token %}

                                <div class="mb-3">
                                    <label for="{{ form.username.id_for_label }}" class="form-label">用户名</label>
                                    {{ form.username }}
                                </div>

                                <div class="mb-3">
                                    <label for="{{ form.email.id_for_label }}" class="form-label">邮箱</label>
                                    {{ form.email }}
                                </div>

                                <div class="mb-3">
                                    <label for="{{ form.phone.id_for_label }}" class="form-label">手机号码</label>
                                    {{ form.phone }}
                                </div>

                                {% if user.user_type == 'agency' and agency %}
                                <div class="mb-3">
                                    <label for="id_agency_name" class="form-label">旅行社名称</label>
                                    <input type="text" name="agency_name" value="{{ agency.agency_name }}" id="id_agency_name" class="form-control">
                                </div>
                                {% endif %}

                                <div class="mb-3">
                                    <button type="submit" class="btn btn-primary">保存修改</button>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

{% block extra_js %}
<script>
    // 头像上传处理
    document.getElementById('id_profile_pic').addEventListener('change', function() {
        const file = this.files[0];
        if (file) {
            const formData = new FormData(document.getElementById('profile-pic-form'));

            fetch('{% url "update_profile_pic" %}', {
                method: 'POST',
                body: formData,
                headers: {
                    'X-CSRFToken': '{{ csrf_token }}'
                },
                credentials: 'same-origin'
            })
            .then(response => response.json())
            .then(data => {
                if (data.success) {
                    document.getElementById('profile-pic').src = data.image_url;
                    // 更新顶部导航栏的头像
                    if (document.getElementById('nav-profile-pic')) {
                        document.getElementById('nav-profile-pic').src = data.image_url;
                    }
                }
            });
        }
    });
</script>
{% endblock %}
{% endblock %}

(4)talkwalk/templates/accounts/register.html:
<!-- templates/accounts/register.html -->
{% extends 'base.html' %}

{% block title %}注册 - Talk&Walk{% endblock %}

{% block content %}
<div class="container login-container">
    <div class="text-center mb-4">
        <h1 class="display-4">Talk&Walk</h1>
        <p class="lead">旅行社管理系统</p>
    </div>

    <div class="card auth-card">
        <div class="card-header">
            用户注册
        </div>
        <div class="card-body">
            {% if form.errors %}
                <div class="alert alert-danger">
                    请修正以下错误:
                    {{ form.errors }}
                </div>
            {% endif %}

            <form method="post" novalidate>
                {% csrf_token %}
                <div class="form-group">
                    <label for="{{ form.username.id_for_label }}">用户名</label>
                    {{ form.username }}
                </div>
                <div class="form-group">
                    <label for="{{ form.email.id_for_label }}">邮箱</label>
                    {{ form.email }}
                </div>
                <div class="form-group">
                    <label for="phone">电话号码</label>
                    <input type="tel" id="phone" name="phone" class="form-control" required>
                </div>
                <div class="form-group">
                    <label for="{{ form.user_type.id_for_label }}">用户类型</label>
                    {{ form.user_type }}
                </div>

                <!-- 姓名/旅行社名字段 - 会根据用户类型动态变化 -->
                <div class="form-group" id="name-field">
                    <label for="name" id="name-label">姓名</label>
                    <input type="text" id="name" name="name" class="form-control" required>
                </div>

                <div class="form-group">
                    <label for="{{ form.password1.id_for_label }}">密码</label>
                    {{ form.password1 }}
                    <!-- 已移除密码复杂度提示 -->
                </div>
                <div class="form-group">
                    <label for="{{ form.password2.id_for_label }}">确认密码</label>
                    {{ form.password2 }}
                </div>
                <div class="d-grid gap-2">
                    <button type="submit" class="btn btn-primary btn-lg">注册</button>
                </div>
            </form>

            <div class="text-center mt-4">
                <p>已有账号？<a href="{% url 'login' %}">立即登录</a></p>
            </div>
        </div>
    </div>
</div>

<script>
    // 根据用户类型动态更改姓名/旅行社名称标签
    document.addEventListener('DOMContentLoaded', function() {
        const userTypeSelect = document.getElementById('id_user_type');
        const nameLabel = document.getElementById('name-label');

        function updateNameLabel() {
            if (userTypeSelect.value === 'agency') {
                nameLabel.textContent = '旅行社名称';
            } else {
                nameLabel.textContent = '姓名';
            }
        }

        // 初始调用一次
        updateNameLabel();

        // 监听用户类型变化
        userTypeSelect.addEventListener('change', updateNameLabel);
    });
</script>
{% endblock %}
(5)talkwalk/templates/accounts/register_success.html:
<!-- templates/accounts/register_success.html -->
{% extends 'base.html' %}

{% block title %}注册成功 - Talk&Walk{% endblock %}

{% block content %}
<div class="container login-container">
    <div class="text-center mb-4">
        <h1 class="display-4">Talk&Walk</h1>
        <p class="lead">旅行社管理系统</p>
    </div>

    <div class="card auth-card">
        <div class="card-header bg-success text-white">
            注册成功
        </div>
        <div class="card-body">
            <div class="text-center">
                <i class="fas fa-check-circle fa-4x text-success mb-3"></i>
                <h3 class="mb-4">恭喜！您的账号已创建成功</h3>
                <p>用户名: <strong>{{ username }}</strong></p>
                <p>将在 <span id="countdown">{{ countdown }}</span> 秒后自动跳转到登录页面</p>
                <div class="progress mb-3">
                    <div class="progress-bar bg-success" role="progressbar" style="width: 100%"></div>
                </div>
                <a href="{% url 'login' %}" class="btn btn-primary">立即登录</a>
            </div>
        </div>
    </div>
</div>

<script>
    // 倒计时功能
    let countdownElement = document.getElementById('countdown');
    let seconds = parseInt(countdownElement.textContent);
    let progressBar = document.querySelector('.progress-bar');

    function updateCountdown() {
        seconds -= 1;
        countdownElement.textContent = seconds;
        progressBar.style.width = (seconds / {{ countdown }} * 100) + '%';

        if (seconds <= 0) {
            clearInterval(interval);
            window.location.href = "{% url 'login' %}";
        }
    }

    let interval = setInterval(updateCountdown, 1000);
</script>
{% endblock %}

(6)talkwalk/templates/agency/analytics.html:
{% extends 'base.html' %}

{% block title %}统计分析 - Talk&Walk{% endblock %}

{% block extra_css %}
<style>
    .stats-card {
        transition: transform 0.2s;
    }
    
    .stats-card:hover {
        transform: translateY(-5px);
    }
    
    .chart-container {
        height: 300px;
    }
</style>
{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>
                
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>
    
    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:schedule_management' %}">
                        <i class="fa fa-calendar"></i> 行程安排
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>
        
        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h2>统计分析</h2>
                <div>
                    <div class="btn-group">
                        <button type="button" class="btn btn-outline-secondary active">月度</button>
                        <button type="button" class="btn btn-outline-secondary">季度</button>
                        <button type="button" class="btn btn-outline-secondary">年度</button>
                    </div>
                </div>
            </div>
            
            <div class="row mb-4">
                <div class="col-lg-3">
                    <div class="card stats-card">
                        <div class="card-body">
                            <div class="d-flex justify-content-between align-items-center">
                                <div>
                                    <h6 class="text-muted mb-2">总预订量</h6>
                                    <h3 class="mb-0">{{ booking_count }}</h3>
                                </div>
                                <div class="text-primary">
                                    <i class="fa fa-calendar-check fa-3x"></i>
                                </div>
                            </div>
                            <div class="mt-3">
                                <div class="d-flex align-items-center">
                                    <span class="text-success me-2">
                                        <i class="fa fa-arrow-up"></i> 5%
                                    </span>
                                    <span class="text-muted">相比上月</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-3">
                    <div class="card stats-card">
                        <div class="card-body">
                            <div class="d-flex justify-content-between align-items-center">
                                <div>
                                    <h6 class="text-muted mb-2">总收入</h6>
                                    <h3 class="mb-0">¥{{ total_revenue }}</h3>
                                </div>
                                <div class="text-success">
                                    <i class="fa fa-coins fa-3x"></i>
                                </div>
                            </div>
                            <div class="mt-3">
                                <div class="d-flex align-items-center">
                                    <span class="text-success me-2">
                                        <i class="fa fa-arrow-up"></i> 8.2%
                                    </span>
                                    <span class="text-muted">相比上月</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-3">
                    <div class="card stats-card">
                        <div class="card-body">
                            <div class="d-flex justify-content-between align-items-center">
                                <div>
                                    <h6 class="text-muted mb-2">平均评分</h6>
                                    <h3 class="mb-0">4.7 <small class="text-muted">/ 5</small></h3>
                                </div>
                                <div class="text-warning">
                                    <i class="fa fa-star fa-3x"></i>
                                </div>
                            </div>
                            <div class="mt-3">
                                <div class="d-flex align-items-center">
                                    <span class="text-success me-2">
                                        <i class="fa fa-arrow-up"></i> 0.2
                                    </span>
                                    <span class="text-muted">相比上月</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-3">
                    <div class="card stats-card">
                        <div class="card-body">
                            <div class="d-flex justify-content-between align-items-center">
                                <div>
                                    <h6 class="text-muted mb-2">总游客数</h6>
                                    <h3 class="mb-0">250</h3>
                                </div>
                                <div class="text-info">
                                    <i class="fa fa-users fa-3x"></i>
                                </div>
                            </div>
                            <div class="mt-3">
                                <div class="d-flex align-items-center">
                                    <span class="text-success me-2">
                                        <i class="fa fa-arrow-up"></i> 12%
                                    </span>
                                    <span class="text-muted">相比上月</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="row mb-4">
                <div class="col-lg-8">
                    <div class="card">
                        <div class="card-header d-flex justify-content-between align-items-center">
                            <h5 class="mb-0">月度收入趋势</h5>
                            <div class="btn-group btn-group-sm">
                                <button type="button" class="btn btn-outline-secondary active">收入</button>
                                <button type="button" class="btn btn-outline-secondary">预订量</button>
                            </div>
                        </div>
                        <div class="card-body">
                            <div class="chart-container" id="revenue-chart"></div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4">
                    <div class="card">
                        <div class="card-header">
                            <h5 class="mb-0">路线受欢迎度</h5>
                        </div>
                        <div class="card-body">
                            <div class="chart-container" id="popularity-chart"></div>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="card">
                <div class="card-header">
                    <h5 class="mb-0">路线统计</h5>
                </div>
                <div class="card-body">
                    <div class="table-responsive">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th>路线名称</th>
                                    <th>预订量</th>
                                    <th>单价</th>
                                    <th>收入</th>
                                    <th>评分</th>
                                    <th>趋势</th>
                                </tr>
                            </thead>
                            <tbody>
                                {% for stat in route_stats %}
                                <tr>
                                    <td>{{ stat.name }}</td>
                                    <td>{{ stat.bookings }}</td>
                                    <td>¥{{ stat.price }}</td>
                                    <td>¥{{ stat.revenue }}</td>
                                    <td>
                                        <div class="text-warning">
                                            <i class="fa fa-star"></i>
                                            <i class="fa fa-star"></i>
                                            <i class="fa fa-star"></i>
                                            <i class="fa fa-star"></i>
                                            <i class="fa fa-star-half-alt"></i>
                                            4.5
                                        </div>
                                    </td>
                                    <td>
                                        <span class="text-success">
                                            <i class="fa fa-arrow-trend-up"></i> 5.2%
                                        </span>
                                    </td>
                                </tr>
                                {% empty %}
                                <tr>
                                    <td colspan="6" class="text-center">暂无数据</td>
                                </tr>
                                {% endfor %}
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

{% block extra_js %}
<!-- 引入Chart.js绘图库 -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
    document.addEventListener('DOMContentLoaded', function() {
        // 收入趋势图
        const revenueChart = new Chart(
            document.getElementById('revenue-chart'),
            {
                type: 'line',
                data: {
                    labels: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'],
                    datasets: [{
                        label: '月度收入(元)',
                        data: [5000, 7500, 12000, 15000, 18000, 25000, 30000, 35000, 28000, 20000, 15000, 12000],
                        borderColor: '#3498db',
                        backgroundColor: 'rgba(52, 152, 219, 0.1)',
                        fill: true,
                        tension: 0.3
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            }
        );
        
        // 路线受欢迎度饼图
        const popularityChart = new Chart(
            document.getElementById('popularity-chart'),
            {
                type: 'pie',
                data: {
                    labels: ['长城一日游', '故宫博物院', '颐和园', '圆明园', '其他'],
                    datasets: [{
                        data: [30, 25, 20, 15, 10],
                        backgroundColor: [
                            '#3498db',
                            '#2ecc71',
                            '#f1c40f',
                            '#e74c3c',
                            '#95a5a6'
                        ]
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'right'
                        }
                    }
                }
            }
        );
    });
</script>
{% endblock %}
{% endblock %}

(7)talkwalk/templates/agency/booking_management.html:
{% extends 'base.html' %}

{% block title %}预订管理 - Talk&Walk{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>
                
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>
    
    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:schedule_management' %}">
                        <i class="fa fa-calendar"></i> 行程安排
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>
        
        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h2>预订管理</h2>
            </div>
            
            <div class="card">
                <div class="card-body">
                    <ul class="nav nav-tabs mb-3">
                        <li class="nav-item">
                            <a class="nav-link active" data-bs-toggle="tab" href="#confirmed-tab">已确认预订</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" data-bs-toggle="tab" href="#waitlist-tab">候补名单</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" data-bs-toggle="tab" href="#cancelled-tab">已取消预订</a>
                        </li>
                    </ul>
                    
                    <div class="tab-content">
                        <!-- 已确认预订 -->
                        <div class="tab-pane fade show active" id="confirmed-tab">
                            <div class="table-responsive">
                                <table class="table table-hover">
                                    <thead>
                                        <tr>
                                            <th>游客姓名</th>
                                            <th>路线名称</th>
                                            <th>行程日期</th>
                                            <th>预订时间</th>
                                            <th>状态</th>
                                            <th>操作</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        {% for booking in confirmed_bookings %}
                                        <tr>
                                            <td>{{ booking.tourist.user.username }}</td>
                                            <td>{{ booking.schedule.route.name }}</td>
                                            <td>{{ booking.schedule.start_date|date:"Y-m-d" }}</td>
                                            <td>{{ booking.booking_date|date:"Y-m-d H:i" }}</td>
                                            <td>
                                                <span class="badge bg-success">已确认</span>
                                            </td>
                                            <td>
                                                <button class="btn btn-sm btn-outline-primary" onclick="viewBookingDetails({{ booking.id }})">
                                                    <i class="fa fa-eye"></i>
                                                </button>
                                                <button class="btn btn-sm btn-outline-danger" onclick="cancelBooking({{ booking.id }})">
                                                    <i class="fa fa-times"></i>
                                                </button>
                                            </td>
                                        </tr>
                                        {% empty %}
                                        <tr>
                                            <td colspan="6" class="text-center">暂无已确认的预订</td>
                                        </tr>
                                        {% endfor %}
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        
                        <!-- 候补名单 -->
                        <div class="tab-pane fade" id="waitlist-tab">
                            <div class="table-responsive">
                                <table class="table table-hover">
                                    <thead>
                                        <tr>
                                            <th>游客姓名</th>
                                            <th>路线名称</th>
                                            <th>行程日期</th>
                                            <th>预订时间</th>
                                            <th>申请理由</th>
                                            <th>操作</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        {% for booking in waitlist_bookings %}
                                        <tr>
                                            <td>{{ booking.tourist.user.username }}</td>
                                            <td>{{ booking.schedule.route.name }}</td>
                                            <td>{{ booking.schedule.start_date|date:"Y-m-d" }}</td>
                                            <td>{{ booking.booking_date|date:"Y-m-d H:i" }}</td>
                                            <td>{{ booking.reason|truncatechars:30 }}</td>
                                            <td>
                                                <button class="btn btn-sm btn-outline-success" onclick="confirmBooking({{ booking.id }})">
                                                    <i class="fa fa-check"></i>
                                                </button>
                                                <button class="btn btn-sm btn-outline-danger" onclick="rejectBooking({{ booking.id }})">
                                                    <i class="fa fa-times"></i>
                                                </button>
                                            </td>
                                        </tr>
                                        {% empty %}
                                        <tr>
                                            <td colspan="6" class="text-center">候补名单为空</td>
                                        </tr>
                                        {% endfor %}
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        
                        <!-- 已取消预订 -->
                        <div class="tab-pane fade" id="cancelled-tab">
                            <div class="table-responsive">
                                <table class="table table-hover">
                                    <thead>
                                        <tr>
                                            <th>游客姓名</th>
                                            <th>路线名称</th>
                                            <th>行程日期</th>
                                            <th>预订时间</th>
                                            <th>取消时间</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr>
                                            <td colspan="5" class="text-center">暂无取消的预订</td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    function viewBookingDetails(bookingId) {
        // 查看预订详情
        alert('查看预订详情功能待实现');
    }
    
    function cancelBooking(bookingId) {
        if (confirm('确定要取消这个预订吗？')) {
            // 取消预订
            alert('取消预订功能待实现');
        }
    }
    
    function confirmBooking(bookingId) {
        if (confirm('确定将此游客从候补名单添加到正式名单吗？')) {
            // 确认预订
            alert('确认预订功能待实现');
        }
    }
    
    function rejectBooking(bookingId) {
        if (confirm('确定要拒绝这个预订申请吗？')) {
            // 拒绝预订
            alert('拒绝预订功能待实现');
        }
    }
</script>
{% endblock %}

(8)talkwalk/templates/agency/dashboard.html:
<!-- templates/agency/dashboard.html -->
{% extends 'base.html' %}

{% block title %}总览 - Talk&Walk{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>

    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:schedule_management' %}">
                        <i class="fa fa-calendar"></i> 行程安排
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>

        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="row mb-4">
                <div class="col-lg-3">
                    <div class="card stats-card" onclick="window.location.href='{% url 'agency:guide_list' %}'">
                        <div class="card-body">
                            <div class="d-flex justify-content-between align-items-center">
                                <div>
                                    <h6 class="text-muted mb-2">导游总数</h6>
                                    <h3 class="mb-0">{{ guide_count }}</h3>
                                </div>
                                <div class="text-primary">
                                    <i class="fa fa-users fa-3x"></i>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-3">
                    <div class="card stats-card" onclick="window.location.href='{% url 'agency:booking_management' %}'">
                        <div class="card-body">
                            <div class="d-flex justify-content-between align-items-center">
                                <div>
                                    <h6 class="text-muted mb-2">预订总数</h6>
                                    <h3 class="mb-0">{{ booking_count }}</h3>
                                </div>
                                <div class="text-success">
                                    <i class="fa fa-calendar-check fa-3x"></i>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-3">
                    <div class="card stats-card" onclick="window.location.href='{% url 'agency:analytics' %}'">
                        <div class="card-body">
                            <div class="d-flex justify-content-between align-items-center">
                                <div>
                                    <h6 class="text-muted mb-2">收入总额</h6>
                                    <h3 class="mb-0">¥{{ total_revenue }}</h3>
                                </div>
                                <div class="text-warning">
                                    <i class="fa fa-coins fa-3x"></i>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-3">
                    <div class="card stats-card" onclick="window.location.href='{% url 'agency:route_management' %}'">
                        <div class="card-body">
                            <div class="d-flex justify-content-between align-items-center">
                                <div>
                                    <h6 class="text-muted mb-2">线路数量</h6>
                                    <h3 class="mb-0">{{ route_count }}</h3>
                                </div>
                                <div class="text-danger">
                                    <i class="fa fa-map fa-3x"></i>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="row">
                <div class="col-lg-12">
                    <div class="card">
                        <div class="card-header d-flex justify-content-between align-items-center">
                            <h5 class="mb-0">近期行程</h5>
                            <a href="{% url 'agency:schedule_management' %}" class="btn btn-sm btn-outline-primary">查看全部</a>
                        </div>
                        <div class="card-body">
                            <div class="table-responsive">
                                <table class="table table-hover">
                                    <thead>
                                        <tr>
                                            <th>行程名称</th>
                                            <th>出发时间</th>
                                            <th>结束时间</th>
                                            <th>导游</th>
                                            <th>报名人数</th>
                                            <th>状态</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        {% for schedule in recent_schedules %}
                                        <tr>
                                            <td>{{ schedule.route.name }}</td>
                                            <td>{{ schedule.start_date }}</td>
                                            <td>{{ schedule.end_date }}</td>
                                            <td>
                                                {% if schedule.guide %}
                                                    {{ schedule.guide.user.username }}
                                                {% else %}
                                                    <span class="text-muted">未分配</span>
                                                {% endif %}
                                            </td>
                                            <td>{{ schedule.get_booking_count }}/{{ schedule.max_tourists }}</td>
                                            <td>
                                                {% if schedule.is_active %}
                                                    <span class="badge bg-success">进行中</span>
                                                {% else %}
                                                    <span class="badge bg-secondary">已结束</span>
                                                {% endif %}
                                            </td>
                                        </tr>
                                        {% empty %}
                                        <tr>
                                            <td colspan="6" class="text-center">暂无近期行程</td>
                                        </tr>
                                        {% endfor %}
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}

(9)talkwalk/templates/agency/guide_list.html:
<!-- talkwalk/templates/agency/guide_list.html -->
{% extends 'base.html' %}

{% block title %}导游管理 - Talk&Walk{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>

    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:schedule_management' %}">
                        <i class="fa fa-calendar"></i> 行程安排
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>

        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h2>导游管理</h2>
                <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#availableGuidesModal">
                    <i class="fa fa-user-plus me-2"></i>聘请导游
                </button>
            </div>

            <div class="row">
                <!-- 当前导游列表 -->
                <div class="col-lg-8">
                    <div class="card">
                        <div class="card-header">
                            <h5 class="mb-0">现有导游</h5>
                        </div>
                        <div class="card-body">
                            <div class="table-responsive">
                                <table class="table table-hover">
                                    <thead>
                                        <tr>
                                            <th>姓名</th>
                                            <th>导游证号</th>
                                            <th>从业年限</th>
                                            <th>评分</th>
                                            <th>工资</th>
                                            <th>投诉次数</th>
                                            <th>操作</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        {% for guide in guides %}
                                        <tr>
                                            <td>{{ guide.user.username }}</td>
                                            <td>{{ guide.license_number }}</td>
                                            <td>{{ guide.experience_years }}年</td>
                                            <td>
                                                <div class="text-warning">
                                                    {% for i in "12345" %}
                                                        {% if forloop.counter <= guide.rating %}
                                                            <i class="fa fa-star"></i>
                                                        {% elif forloop.counter <= guide.rating|add:"0.5" %}
                                                            <i class="fa fa-star-half-alt"></i>
                                                        {% else %}
                                                            <i class="far fa-star"></i>
                                                        {% endif %}
                                                    {% endfor %}
                                                    {{ guide.rating }}
                                                </div>
                                            </td>
                                            <td>¥{{ guide.salary }}</td>
                                            <td>{{ guide.complaint_count }}</td>
                                            <td>
                                                <button class="btn btn-sm btn-outline-primary me-1" onclick="viewGuideDetails({{ guide.id }})">
                                                    <i class="fa fa-eye"></i>
                                                </button>
                                                <button class="btn btn-sm btn-outline-danger" onclick="dismissGuide({{ guide.id }})">
                                                    <i class="fa fa-times"></i>
                                                </button>
                                            </td>
                                        </tr>
                                        {% empty %}
                                        <tr>
                                            <td colspan="7" class="text-center">暂无导游，请点击"聘请导游"按钮添加。</td>
                                        </tr>
                                        {% endfor %}
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 导游申请列表 -->
                <div class="col-lg-4">
                    <div class="card">
                        <div class="card-header">
                            <h5 class="mb-0">导游申请</h5>
                        </div>
                        <div class="card-body">
                            <div class="list-group">
                                {% for application in guide_applications %}
                                    {% if application.status == 'pending' %}
                                    <div class="list-group-item">
                                        <div class="d-flex justify-content-between align-items-center">
                                            <div>
                                                <h6 class="mb-1">{{ application.guide.user.username }}</h6>
                                                <small>导游证号: {{ application.guide.license_number }}</small>
                                            </div>
                                            <div>
                                                <button class="btn btn-sm btn-success me-1" onclick="acceptGuideApplication({{ application.id }})">
                                                    <i class="fa fa-check"></i>
                                                </button>
                                                <button class="btn btn-sm btn-danger" onclick="rejectGuideApplication({{ application.id }})">
                                                    <i class="fa fa-times"></i>
                                                </button>
                                            </div>
                                        </div>
                                        <p class="mb-1 text-muted">{{ application.message }}</p>
                                        <small>申请时间: {{ application.created_at|date:"Y-m-d H:i" }}</small>
                                    </div>
                                    {% endif %}
                                {% empty %}
                                <div class="text-center py-4">
                                    <i class="fa fa-inbox fa-2x text-muted mb-2"></i>
                                    <p class="text-muted">暂无导游申请</p>
                                </div>
                                {% endfor %}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- 可用导游模态框 -->
<div class="modal fade" id="availableGuidesModal" tabindex="-1">
    <div class="modal-dialog modal-lg">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title">聘请导游</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <div id="loading-guides" class="text-center py-4">
                    <div class="spinner-border text-primary" role="status">
                        <span class="visually-hidden">加载中...</span>
                    </div>
                    <p class="mt-2">正在加载可聘请导游...</p>
                </div>

                <div id="available-guides-list" style="display:none;">
                    <div class="table-responsive">
                        <table class="table table-hover">
                            <thead>
                                <tr>
                                    <th>姓名</th>
                                    <th>导游证号</th>
                                    <th>从业年限</th>
                                    <th>操作</th>
                                </tr>
                            </thead>
                            <tbody id="guides-table-body">
                                <!-- 动态加载内容 -->
                            </tbody>
                        </table>
                    </div>
                    <div id="no-guides-message" class="text-center py-4" style="display:none;">
                        <i class="fa fa-user-slash fa-2x text-muted mb-2"></i>
                        <p class="text-muted">目前没有可聘请的导游</p>
                    </div>
                </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">关闭</button>
            </div>
        </div>
    </div>
</div>

<!-- 邀请信息模态框 -->
<div class="modal fade" id="inviteMessageModal" tabindex="-1">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title">发送邀请信息</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <form id="inviteMessageForm">
                    <input type="hidden" id="selected-guide-id">
                    <div class="mb-3">
                        <label for="guide-name" class="form-label">导游姓名</label>
                        <input type="text" class="form-control" id="guide-name" readonly>
                    </div>
                    <div class="mb-3">
                        <label for="invite-message" class="form-label">邀请信息</label>
                        <textarea class="form-control" id="invite-message" rows="3" placeholder="请输入邀请信息..."></textarea>
                    </div>
                </form>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">取消</button>
                <button type="button" class="btn btn-primary" id="send-invite-btn">发送邀请</button>
            </div>
        </div>
    </div>
</div>

<script>
    // 获取可聘请的导游列表
    function loadAvailableGuides() {
        const loadingElement = document.getElementById('loading-guides');
        const guidesListElement = document.getElementById('available-guides-list');
        const noGuidesMessage = document.getElementById('no-guides-message');
        const guidesTableBody = document.getElementById('guides-table-body');

        loadingElement.style.display = 'block';
        guidesListElement.style.display = 'none';
        noGuidesMessage.style.display = 'none';

        fetch('{% url "agency:get_available_guides" %}')
            .then(response => response.json())
            .then(data => {
                loadingElement.style.display = 'none';
                guidesListElement.style.display = 'block';

                if (data.success && data.guides.length > 0) {
                    let html = '';
                    data.guides.forEach(guide => {
                        html += `
                            <tr>
                                <td>${guide.user__username}</td>
                                <td>${guide.license_number || '未提供'}</td>
                                <td>${guide.experience_years || 0}年</td>
                                <td>
                                    <button class="btn btn-sm btn-primary" onclick="showInviteForm(${guide.id}, '${guide.user__username}')">
                                        <i class="fa fa-user-plus me-1"></i> 聘请
                                    </button>
                                </td>
                            </tr>
                        `;
                    });
                    guidesTableBody.innerHTML = html;
                } else {
                    noGuidesMessage.style.display = 'block';
                }
            })
            .catch(error => {
                console.error('Error:', error);
                loadingElement.style.display = 'none';
                alert('获取导游列表失败，请重试');
            });
    }

    // 显示邀请表单
    function showInviteForm(guideId, guideName) {
        document.getElementById('selected-guide-id').value = guideId;
        document.getElementById('guide-name').value = guideName;

        // 隐藏第一个模态框，显示第二个
        const availableGuidesModal = bootstrap.Modal.getInstance(document.getElementById('availableGuidesModal'));
        availableGuidesModal.hide();

        // 显示邀请表单模态框
        const inviteMessageModal = new bootstrap.Modal(document.getElementById('inviteMessageModal'));
        inviteMessageModal.show();
    }

    // 发送邀请
    function sendInvitation() {
        const guideId = document.getElementById('selected-guide-id').value;
        const message = document.getElementById('invite-message').value;

        if (!guideId) {
            alert('请先选择导游');
            return;
        }

        fetch('{% url "agency:invite_guide" %}', {
            method: 'POST',
            headers: {
                'X-CSRFToken': '{{ csrf_token }}',
                'Content-Type': 'application/x-www-form-urlencoded',
            },
            body: `guide_id=${guideId}&message=${encodeURIComponent(message)}`
        })
        .then(response => response.json())
        .then(data => {
            if (data.success) {
                alert('邀请已发送！');

                // 关闭邀请表单模态框
                const inviteMessageModal = bootstrap.Modal.getInstance(document.getElementById('inviteMessageModal'));
                inviteMessageModal.hide();

                // 重新加载导游列表
                loadAvailableGuides();
            } else {
                alert('发送邀请失败: ' + (data.error || '未知错误'));
            }
        })
        .catch(error => {
            console.error('Error:', error);
            alert('发送邀请时出错，请重试');
        });
    }

    // 查看导游详情
    function viewGuideDetails(guideId) {
        // 功能待实现
        alert('查看导游详情功能待实现');
    }

    // 解雇导游
    function dismissGuide(guideId) {
        if (confirm('确定要解雇这位导游吗？')) {
            // 功能待实现
            alert('解雇导游功能待实现');
        }
    }

    // 接受导游申请
    function acceptGuideApplication(applicationId) {
        // 功能待实现
        alert('接受申请功能待实现');
    }

    // 拒绝导游申请
    function rejectGuideApplication(applicationId) {
        // 功能待实现
        alert('拒绝申请功能待实现');
    }

    // 当模态框显示时加载导游列表
    document.addEventListener('DOMContentLoaded', function() {
        const availableGuidesModal = document.getElementById('availableGuidesModal');
        availableGuidesModal.addEventListener('show.bs.modal', loadAvailableGuides);

        // 发送邀请按钮点击事件
        document.getElementById('send-invite-btn').addEventListener('click', sendInvitation);
    });
</script>
{% endblock %}

(10)talkwalk/templates/agency/itinerary_list.html:
<!-- talkwalk/templates/agency/itinerary_list.html -->
{% extends 'base.html' %}

{% block title %}行程管理 - Talk&Walk{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>

    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:itinerary_list' %}">
                        <i class="fa fa-calendar"></i> 行程管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:tourism_policies' %}">
                        <i class="fa fa-file-alt"></i> 旅游政策
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>

        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h2>行程管理</h2>
                <a href="{% url 'agency:itinerary_create' %}" class="btn btn-primary">
                    <i class="fa fa-plus me-2"></i>创建新行程
                </a>
            </div>

            <div class="card">
                <div class="card-body">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="table-responsive">
                                <table class="table table-hover">
                                    <thead>
                                        <tr>
                                            <th>行程名称</th>
                                            <th>价格</th>
                                            <th>路线数量</th>
                                            <th>创建时间</th>
                                            <th>操作</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        {% for itinerary in itineraries %}
                                        <tr>
                                            <td>{{ itinerary.name }}</td>
                                            <td>¥{{ itinerary.price }}</td>
                                            <td>{{ itinerary.routes.count }}</td>
                                            <td>{{ itinerary.created_at|date:"Y-m-d" }}</td>
                                            <td>
                                                <a href="{% url 'agency:itinerary_detail' itinerary.id %}" class="btn btn-sm btn-outline-primary me-1">
                                                    <i class="fa fa-eye"></i> 查看
                                                </a>
                                                <a href="#" class="btn btn-sm btn-outline-secondary me-1">
                                                    <i class="fa fa-edit"></i> 编辑
                                                </a>
                                                <button class="btn btn-sm btn-outline-danger" onclick="deleteItinerary({{ itinerary.id }})">
                                                    <i class="fa fa-trash"></i>
                                                </button>
                                            </td>
                                        </tr>
                                        {% empty %}
                                        <tr>
                                            <td colspan="5" class="text-center py-4">
                                                <div>
                                                    <i class="fa fa-route fa-3x text-muted mb-3"></i>
                                                    <h5 class="text-muted">暂无行程</h5>
                                                    <p class="text-muted">点击右上角"创建新行程"按钮创建您的第一个行程</p>
                                                </div>
                                            </td>
                                        </tr>
                                        {% endfor %}
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    function deleteItinerary(itineraryId) {
        if (confirm('确定要删除这个行程吗？此操作不可恢复。')) {
            // 后端处理功能待实现
            alert('删除行程功能待实现');
        }
    }
</script>
{% endblock %}

(11)talkwalk/templates/agency/notifications.html:
{% extends 'base.html' %}

{% block title %}通知 - Talk&Walk{% endblock %}

{% block extra_css %}
<style>
    .chat-container {
        height: calc(100vh - 200px);
        display: flex;
        flex-direction: column;
    }
    
    .chat-sidebar {
        width: 280px;
        border-right: 1px solid #eee;
        overflow-y: auto;
    }
    
    .chat-main {
        flex: 1;
        display: flex;
        flex-direction: column;
    }
    
    .chat-header {
        padding: 15px;
        border-bottom: 1px solid #eee;
    }
    
    .chat-messages {
        flex: 1;
        padding: 15px;
        overflow-y: auto;
    }
    
    .chat-input {
        padding: 15px;
        border-top: 1px solid #eee;
    }
    
    .chat-contact {
        padding: 10px 15px;
        border-bottom: 1px solid #f8f9fa;
        cursor: pointer;
    }
    
    .chat-contact:hover {
        background-color: #f8f9fa;
    }
    
    .chat-contact.active {
        background-color: #e9f5ff;
        border-left: 3px solid var(--primary-color);
    }
    
    .message {
        margin-bottom: 15px;
        max-width: 70%;
    }
    
    .message-received {
        align-self: flex-start;
    }
    
    .message-sent {
        align-self: flex-end;
        background-color: #dcf8c6;
    }
    
    .message-content {
        padding: 10px;
        border-radius: 10px;
        background-color: #f1f0f0;
        display: inline-block;
    }
    
    .message-sent .message-content {
        background-color: #dcf8c6;
    }
    
    .message-info {
        font-size: 0.8rem;
        color: #777;
        margin-top: 5px;
    }
    
    .message-avatar {
        width: 30px;
        height: 30px;
        border-radius: 50%;
        margin-right: 10px;
    }
</style>
{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>
                
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>
    
    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:itinerary_list' %}">
                        <i class="fa fa-calendar"></i> 行程管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:tourism_policies' %}">
                        <i class="fa fa-file-alt"></i> 旅游政策
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>

        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h2>消息通知</h2>
            </div>

            <div class="card">
                <div class="card-body p-0">
                    <div class="chat-container d-flex">
                        <!-- 联系人列表 -->
                        <div class="chat-sidebar">
                            <div class="p-3">
                                <div class="input-group">
                                    <input type="text" class="form-control" placeholder="搜索...">
                                    <button class="btn btn-outline-secondary" type="button">
                                        <i class="fa fa-search"></i>
                                    </button>
                                </div>
                            </div>

                            <div class="chat-contacts">
                                <!-- 存储当前聊天对象ID的隐藏字段 -->
                                <input type="hidden" id="current-chat-id" value="1">

                                <!-- 置顶的行业群组 -->
                                <div class="chat-contact active" data-chat-id="1" onclick="selectChat(this, 1)">
                                    <div class="d-flex align-items-center">
                                        <div class="flex-shrink-0">
                                            <div class="avatar d-flex align-items-center justify-content-center bg-primary text-white">
                                                <i class="fa fa-users"></i>
                                            </div>
                                        </div>
                                        <div class="flex-grow-1 ms-3">
                                            <h6 class="mb-0">旅游行业群组</h6>
                                            <small class="text-muted">文旅局、各旅行社</small>
                                        </div>
                                    </div>
                                </div>

                                <!-- 导游列表 -->
                                <div class="p-2 bg-light">
                                    <small class="text-muted">导游</small>
                                </div>
                                {% for guide in agency.guides.all %}
                                <div class="chat-contact" data-chat-id="{{ guide.user.id }}" onclick="selectChat(this, {{ guide.user.id }})">
                                    <div class="d-flex align-items-center">
                                        <div class="flex-shrink-0">
                                            {% if guide.user.profile_pic %}
                                                <img src="{{ guide.user.profile_pic.url }}" class="message-avatar">
                                            {% else %}
                                                <div class="avatar d-flex align-items-center justify-content-center bg-info text-white">
                                                    {{ guide.user.username|first }}
                                                </div>
                                            {% endif %}
                                        </div>
                                        <div class="flex-grow-1 ms-3">
                                            <h6 class="mb-0">{{ guide.user.username }}</h6>
                                            <small class="text-muted">导游</small>
                                        </div>
                                    </div>
                                </div>
                                {% empty %}
                                <div class="p-3 text-center text-muted">
                                    <small>暂无导游</small>
                                </div>
                                {% endfor %}

                                <!-- 游客列表 -->
                                <div class="p-2 bg-light">
                                    <small class="text-muted">联系过的游客</small>
                                </div>
                                <div class="p-3 text-center text-muted">
                                    <small>暂无游客联系记录</small>
                                </div>
                            </div>
                        </div>

                        <!-- 聊天主区域 -->
                        <div class="chat-main">
                            <!-- 聊天头部 -->
                            <div class="chat-header">
                                <div class="d-flex align-items-center justify-content-between">
                                    <div class="d-flex align-items-center">
                                        <div class="flex-shrink-0">
                                            <div class="avatar d-flex align-items-center justify-content-center bg-primary text-white">
                                                <i class="fa fa-users"></i>
                                            </div>
                                        </div>
                                        <div class="flex-grow-1 ms-3">
                                            <h5 class="mb-0" id="chat-title">旅游行业群组</h5>
                                            <small class="text-muted" id="chat-subtitle">文旅局、各旅行社</small>
                                        </div>
                                    </div>
                                    <div>
                                        <a href="{% url 'agency:tourism_policies' %}" class="btn btn-sm btn-outline-info">
                                            <i class="fa fa-file-alt me-1"></i> 查看旅游政策
                                        </a>
                                    </div>
                                </div>
                            </div>

                            <!-- 聊天消息 -->
                            <div class="chat-messages d-flex flex-column">
                                <div class="message message-received">
                                    <div class="d-flex">
                                        <div class="avatar message-avatar d-flex align-items-center justify-content-center bg-danger text-white">
                                            局
                                        </div>
                                        <div>
                                            <div class="message-content">
                                                欢迎各旅行社加入行业群组！请大家遵守行业规范，提供优质旅游服务。
                                            </div>
                                            <div class="message-info">
                                                文旅局 · 昨天 15:30
                                            </div>
                                        </div>
                                    </div>
                                </div>

                                <div class="message message-received">
                                    <div class="d-flex">
                                        <div class="avatar message-avatar d-flex align-items-center justify-content-center bg-danger text-white">
                                            局
                                        </div>
                                        <div>
                                            <div class="message-content">
                                                最新旅游政策已发布，请各旅行社查看并遵守执行。详情可在文旅局官方网站查看。
                                            </div>
                                            <div class="message-info">
                                                文旅局 · 今天 09:15
                                            </div>
                                        </div>
                                    </div>
                                </div>

                                <div class="message message-sent align-self-end">
                                    <div class="d-flex flex-row-reverse">
                                        <div class="avatar message-avatar d-flex align-items-center justify-content-center bg-primary text-white">
                                            {{ user.username|first }}
                                        </div>
                                        <div class="text-end">
                                            <div class="message-content">
                                                已收到，我们会严格遵守新政策。
                                            </div>
                                            <div class="message-info">
                                                {{ agency.agency_name }} · 今天 09:20
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <!-- 聊天输入框 -->
                            <div class="chat-input">
                                <form id="message-form">
                                    <div class="d-flex">
                                        <div class="flex-grow-1">
                                            <textarea class="form-control" rows="2" placeholder="输入消息..." id="message-input" required></textarea>
                                        </div>
                                        <div class="ms-2 d-flex align-items-end">
                                            <button type="submit" class="btn btn-primary">
                                                <i class="fa fa-paper-plane"></i> 发送
                                            </button>
                                        </div>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    document.addEventListener('DOMContentLoaded', function() {
        // 消息表单提交
        const messageForm = document.getElementById('message-form');
        const messageInput = document.getElementById('message-input');
        const messagesContainer = document.querySelector('.chat-messages');
        const currentChatId = document.getElementById('current-chat-id');

        if (messageForm) {
            messageForm.addEventListener('submit', function(e) {
                e.preventDefault();

                const message = messageInput.value.trim();
                if (!message) return;

                // 发送消息
                fetch('{% url "agency:send_notification" %}', {
                    method: 'POST',
                    headers: {
                        'X-CSRFToken': '{{ csrf_token }}',
                        'Content-Type': 'application/x-www-form-urlencoded',
                    },
                    body: `receiver_id=${currentChatId.value}&message=${encodeURIComponent(message)}`
                })
                .then(response => response.json())
                .then(data => {
                    if (data.success) {
                        // 添加消息到UI
                        const messageHTML = `
                            <div class="message message-sent align-self-end">
                                <div class="d-flex flex-row-reverse">
                                    <div class="avatar message-avatar d-flex align-items-center justify-content-center bg-primary text-white">
                                        {{ user.username|first }}
                                    </div>
                                    <div class="text-end">
                                        <div class="message-content">
                                            ${message}
                                        </div>
                                        <div class="message-info">
                                            {{ agency.agency_name }} · 刚刚
                                        </div>
                                    </div>
                                </div>
                            </div>
                        `;

                        messagesContainer.innerHTML += messageHTML;
                        messageInput.value = '';

                        // 滚动到底部
                        messagesContainer.scrollTop = messagesContainer.scrollHeight;
                    } else {
                        alert('发送失败: ' + data.error);
                    }
                })
                .catch(error => {
                    console.error('Error:', error);
                    alert('发送出错，请重试');
                });
            });
        }
    });

    // 选择聊天对象
    function selectChat(element, chatId) {
        // 更新当前选中的聊天
        document.querySelectorAll('.chat-contact').forEach(item => {
            item.classList.remove('active');
        });
        element.classList.add('active');

        // 更新隐藏字段
        document.getElementById('current-chat-id').value = chatId;

        // 更新聊天标题
        const title = element.querySelector('h6').textContent;
        const subtitle = element.querySelector('small').textContent;
        document.getElementById('chat-title').textContent = title;
        document.getElementById('chat-subtitle').textContent = subtitle;

        // 这里可以添加加载聊天记录的代码
        // loadChatMessages(chatId);
    }

    // 加载聊天记录
    function loadChatMessages(chatId) {
        // 在实际应用中，这里应该从后端获取聊天记录
        console.log('加载与用户 ID:', chatId, '的聊天记录');
    }
</script>
{% endblock %}

(12)talkwalk/templates/agency/route_edit.html:
<!-- talkwalk/templates/agency/route_edit.html -->
{% extends 'base.html' %}

{% block title %}编辑路线 - {{ route.name }} - Talk&Walk{% endblock %}

{% block extra_css %}
<!-- 高德地图API -->
<link rel="stylesheet" href="https://a.amap.com/jsapi_demos/static/demo-center/css/demo-center.css" />
<style>
    #map-container {
        width: 100%;
        height: 500px;
        position: relative;
    }

    .spot-info-panel {
        max-height: 500px;
        overflow-y: auto;
    }

    .mode-controls {
        position: absolute;
        bottom: 10px;
        left: 10px;
        background: white;
        padding: 10px;
        border-radius: 5px;
        box-shadow: 0 2px 6px rgba(0,0,0,0.3);
        z-index: 100;
    }

    .coordinate-info {
        position: absolute;
        top: 10px;
        right: 10px;
        background: rgba(255,255,255,0.8);
        padding: 5px 10px;
        border-radius: 5px;
        font-size: 12px;
        z-index: 100;
    }

    .search-box {
        position: absolute;
        top: 10px;
        left: 10px;
        width: 300px;
        background: white;
        padding: 10px;
        border-radius: 5px;
        box-shadow: 0 2px 6px rgba(0,0,0,0.3);
        z-index: 100;
    }

    .spot-images {
        display: flex;
        flex-wrap: nowrap;
        overflow-x: auto;
        padding: 10px 0;
        gap: 10px;
    }

    .spot-image {
        width: 100px;
        height: 100px;
        object-fit: cover;
        border-radius: 5px;
        cursor: pointer;
    }

    .spot-image-container {
        position: relative;
    }

    .image-delete-btn {
        position: absolute;
        top: -5px;
        right: -5px;
        background: rgba(255,0,0,0.8);
        color: white;
        border: none;
        border-radius: 50%;
        width: 20px;
        height: 20px;
        font-size: 12px;
        line-height: 1;
        cursor: pointer;
    }

    .search-result {
        position: absolute;
        top: 50px;
        left: 10px;
        width: 300px;
        max-height: 300px;
        overflow-y: auto;
        background: white;
        border-radius: 5px;
        box-shadow: 0 2px 6px rgba(0,0,0,0.3);
        z-index: 101;
    }

    .search-result-item {
        padding: 10px;
        border-bottom: 1px solid #eee;
        cursor: pointer;
    }

    .search-result-item:hover {
        background-color: #f5f5f5;
    }

    .search-result-item h6 {
        margin: 0;
        font-size: 14px;
    }

    .search-result-item p {
        margin: 5px 0 0;
        font-size: 12px;
        color: #666;
    }

    .path-selection-modal .modal-body {
        padding: 0;
    }

    .path-option {
        padding: 15px;
        border-bottom: 1px solid #eee;
        cursor: pointer;
    }

    .path-option:hover {
        background-color: #f5f5f5;
    }

    .path-option.selected {
        background-color: #e3f2fd;
    }

    .path-option h6 {
        margin: 0;
        font-size: 16px;
    }

    .path-option p {
        margin: 5px 0 0;
        font-size: 14px;
    }
</style>
{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>

    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:itinerary_list' %}">
                        <i class="fa fa-calendar"></i> 行程管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:tourism_policies' %}">
                        <i class="fa fa-file-alt"></i> 旅游政策
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>

        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <div>
                    <h2>编辑路线: {{ route.name }}</h2>
                    <p class="text-muted">行程: {{ itinerary.name }}</p>
                </div>
                <div>
                    <button type="button" class="btn btn-success me-2" id="save-route-btn">
                        <i class="fa fa-save me-2"></i>保存路线
                    </button>
                    <a href="{% url 'agency:itinerary_detail' itinerary.id %}" class="btn btn-outline-secondary">
                        <i class="fa fa-arrow-left me-2"></i>返回行程
                    </a>
                </div>
            </div>

            <div class="row">
                <div class="col-lg-12 mb-4">
                    <div class="card">
                        <div class="card-header">
                            基本信息
                        </div>
                        <div class="card-body">
                            <form method="post" id="route-form">
                                {% csrf_token %}
                                <div class="row">
                                    <div class="col-md-6">
                                        <div class="mb-3">
                                            <label for="name" class="form-label">路线名称</label>
                                            <input type="text" class="form-control" id="name" name="name" value="{{ route.name }}" required>
                                        </div>
                                    </div>
                                    <div class="col-md-6">
                                        <div class="mb-3">
                                            <label for="order" class="form-label">排序</label>
                                            <input type="number" class="form-control" id="order" name="order" value="{{ route.order }}" min="1">
                                        </div>
                                    </div>
                                </div>
                                <div class="mb-3">
                                    <label for="description" class="form-label">路线描述</label>
(13)talkwalk/templates/agency/route_management.html:
{% extends 'base.html' %}

{% block title %}路线管理 - Talk&Walk{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>
                
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>
    
    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:schedule_management' %}">
                        <i class="fa fa-calendar"></i> 行程安排
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>
        
        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h2>路线管理</h2>
                <a href="{% url 'routes:route_create' %}" class="btn btn-primary">
                    <i class="fa fa-plus me-2"></i>创建新路线
                </a>
            </div>
            
            <div class="card">
                <div class="card-body">
                    <div class="row">
                        <div class="col-lg-12">
                            <h5 class="mb-4">已创建的路线</h5>
                            <div class="table-responsive">
                                <table class="table table-hover">
                                    <thead>
                                        <tr>
                                            <th>路线名称</th>
                                            <th>描述</th>
                                            <th>价格</th>
                                            <th>创建时间</th>
                                            <th>操作</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        {% for route in routes %}
                                        <tr>
                                            <td>{{ route.name }}</td>
                                            <td>{{ route.description|truncatechars:50 }}</td>
                                            <td>¥{{ route.price }}</td>
                                            <td>{{ route.created_at|date:"Y-m-d" }}</td>
                                            <td>
                                                <a href="{% url 'routes:route_detail' route.id %}" class="btn btn-sm btn-outline-primary me-1">
                                                    <i class="fa fa-eye"></i> 查看
                                                </a>
                                                <a href="{% url 'routes:route_edit' route.id %}" class="btn btn-sm btn-outline-secondary">
                                                    <i class="fa fa-edit"></i> 编辑
                                                </a>
                                            </td>
                                        </tr>
                                        {% empty %}
                                        <tr>
                                            <td colspan="5" class="text-center">
                                                暂无路线，请点击右上角"创建新路线"按钮创建。
                                            </td>
                                        </tr>
                                        {% endfor %}
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="card mt-4">
                <div class="card-header">
                    <h5 class="mb-0">路线地图预览</h5>
                </div>
                <div class="card-body p-0">
                    <div id="map-container" style="height: 400px;">
                        <div class="text-center py-5">
                            <i class="fa fa-map-marked fa-3x text-muted mb-3"></i>
                            <p class="text-muted">选择路线后查看地图预览</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

{% block extra_js %}
<script type="text/javascript" src="https://webapi.amap.com/maps?v=2.0&key={{ api_key }}"></script>
<script>
    // 这里可以添加高德地图相关的JavaScript代码
</script>
{% endblock %}
{% endblock %}

(14)talkwalk/templates/agency/schedule_management.html:
{% extends 'base.html' %}

{% block title %}行程安排 - Talk&Walk{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>
                
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>
    
    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:schedule_management' %}">
                        <i class="fa fa-calendar"></i> 行程安排
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>
        
        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h2>行程安排</h2>
                <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#createScheduleModal">
                    <i class="fa fa-plus me-2"></i>创建新行程
                </button>
            </div>
            
            <div class="card">
                <div class="card-body">
                    <ul class="nav nav-tabs mb-3">
                        <li class="nav-item">
                            <a class="nav-link active" data-bs-toggle="tab" href="#active-tab">进行中行程</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" data-bs-toggle="tab" href="#upcoming-tab">即将开始</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" data-bs-toggle="tab" href="#past-tab">已结束行程</a>
                        </li>
                    </ul>
                    
                    <div class="tab-content">
                        <!-- 进行中行程 -->
                        <div class="tab-pane fade show active" id="active-tab">
                            <div class="table-responsive">
                                <table class="table">
                                    <thead>
                                        <tr>
                                            <th>路线名称</th>
                                            <th>出发日期</th>
                                            <th>结束日期</th>
                                            <th>导游</th>
                                            <th>价格</th>
                                            <th>预订人数</th>
                                            <th>操作</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        {% for schedule in routes.schedules.all %}
                                            {% if schedule.is_active %}
                                            <tr>
                                                <td>{{ schedule.route.name }}</td>
                                                <td>{{ schedule.start_date|date:"Y-m-d" }}</td>
                                                <td>{{ schedule.end_date|date:"Y-m-d" }}</td>
                                                <td>
                                                    {% if schedule.guide %}
                                                        {{ schedule.guide.user.username }}
                                                    {% else %}
                                                        <span class="text-danger">未分配</span>
                                                    {% endif %}
                                                </td>
                                                <td>¥{{ schedule.price }}</td>
                                                <td>{{ schedule.get_booking_count }}/{{ schedule.max_tourists }}</td>
                                                <td>
                                                    <button class="btn btn-sm btn-outline-primary" onclick="editSchedule({{ schedule.id }})">
                                                        <i class="fa fa-edit"></i>
                                                    </button>
                                                    <button class="btn btn-sm btn-outline-secondary" onclick="viewBookings({{ schedule.id }})">
                                                        <i class="fa fa-users"></i>
                                                    </button>
                                                </td>
                                            </tr>
                                            {% endif %}
                                        {% empty %}
                                        <tr>
                                            <td colspan="7" class="text-center">
                                                暂无进行中的行程
                                            </td>
                                        </tr>
                                        {% endfor %}
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        
                        <!-- 即将开始 -->
                        <div class="tab-pane fade" id="upcoming-tab">
                            <div class="table-responsive">
                                <table class="table">
                                    <thead>
                                        <tr>
                                            <th>路线名称</th>
                                            <th>出发日期</th>
                                            <th>结束日期</th>
                                            <th>导游</th>
                                            <th>价格</th>
                                            <th>预订人数</th>
                                            <th>操作</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr>
                                            <td colspan="7" class="text-center">
                                                暂无即将开始的行程
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        
                        <!-- 已结束行程 -->
                        <div class="tab-pane fade" id="past-tab">
                            <div class="table-responsive">
                                <table class="table">
                                    <thead>
                                        <tr>
                                            <th>路线名称</th>
                                            <th>出发日期</th>
                                            <th>结束日期</th>
                                            <th>导游</th>
                                            <th>价格</th>
                                            <th>预订人数</th>
                                            <th>操作</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr>
                                            <td colspan="7" class="text-center">
                                                暂无已结束的行程
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- 创建行程模态框 -->
<div class="modal fade" id="createScheduleModal" tabindex="-1">
    <div class="modal-dialog modal-lg">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title">创建新行程</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <form id="scheduleForm">
                    <div class="row mb-3">
                        <div class="col-md-12">
                            <label for="route_id" class="form-label">选择路线</label>
                            <select class="form-select" id="route_id" required>
                                <option value="" selected disabled>请选择路线</option>
                                {% for route in routes %}
                                    <option value="{{ route.id }}">{{ route.name }}</option>
                                {% endfor %}
                            </select>
                        </div>
                    </div>
                    <div class="row mb-3">
                        <div class="col-md-6">
                            <label for="start_date" class="form-label">出发日期</label>
                            <input type="date" class="form-control" id="start_date" required>
                        </div>
                        <div class="col-md-6">
                            <label for="end_date" class="form-label">结束日期</label>
                            <input type="date" class="form-control" id="end_date" required>
                        </div>
                    </div>
                    <div class="row mb-3">
                        <div class="col-md-6">
                            <label for="price" class="form-label">价格</label>
                            <div class="input-group">
                                <span class="input-group-text">¥</span>
                                <input type="number" class="form-control" id="price" min="0" step="0.01" required>
                            </div>
                        </div>
                        <div class="col-md-6">
                            <label for="max_tourists" class="form-label">最大游客数</label>
                            <input type="number" class="form-control" id="max_tourists" value="20" min="1" required>
                        </div>
                    </div>
                    <div class="mb-3">
                        <label for="guide_id" class="form-label">指派导游</label>
                        <select class="form-select" id="guide_id">
                            <option value="" selected>暂不指派</option>
                            {% for guide in guides %}
                                <option value="{{ guide.id }}">{{ guide.user.username }}</option>
                            {% endfor %}
                        </select>
                    </div>
                </form>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">取消</button>
                <button type="button" class="btn btn-primary" onclick="createSchedule()">创建行程</button>
            </div>
        </div>
    </div>
</div>

<script>
    function editSchedule(scheduleId) {
        // 编辑行程
        alert('编辑行程功能待实现');
    }
    
    function viewBookings(scheduleId) {
        // 查看预订
        alert('查看预订功能待实现');
    }
    
    function createSchedule() {
        // 创建新行程
        alert('创建行程功能待实现');
        $('#createScheduleModal').modal('hide');
    }
</script>
{% endblock %}

(15)talkwalk/templates/agency/tourism_policies.html:
<!-- talkwalk/templates/agency/tourism_policies.html -->
{% extends 'base.html' %}

{% block title %}旅游政策 - Talk&Walk{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                {% if agency %}
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                {% endif %}
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>

    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:schedule_management' %}">
                        <i class="fa fa-calendar"></i> 行程安排
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:tourism_policies' %}">
                        <i class="fa fa-file-alt"></i> 旅游政策
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>

        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h2>旅游政策</h2>
            </div>

            <div class="card">
                <div class="card-body">
                    <div class="row">
                        <div class="col-lg-12">
                            {% for policy in policies %}
                            <div class="policy-item mb-4">
                                <h4>{{ policy.title }}</h4>
                                <div class="d-flex justify-content-between align-items-center mb-2">
                                    <div class="text-muted">
                                        <i class="fa fa-user me-1"></i> {{ policy.publisher }}
                                        <i class="fa fa-calendar ms-3 me-1"></i> {{ policy.publish_date|date:"Y-m-d" }}
                                    </div>
                                    {% if policy.attachment %}
                                    <a href="{{ policy.attachment.url }}" target="_blank" class="btn btn-sm btn-outline-primary">
                                        <i class="fa fa-download me-1"></i> 下载附件
                                    </a>
                                    {% endif %}
                                </div>
                                <div class="card">
                                    <div class="card-body">
                                        {{ policy.content|linebreaks }}
                                    </div>
                                </div>
                            </div>
                            {% empty %}
                            <div class="text-center py-5">
                                <i class="fa fa-file-alt fa-3x text-muted mb-3"></i>
                                <h5 class="text-muted">暂无旅游政策</h5>
                                <p class="text-muted">文旅局尚未发布任何政策</p>
                            </div>
                            {% endfor %}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}

(16)talkwalk/templates/routes/route_detail.html:
<!-- talkwalk/templates/routes/route_detail.html -->
{% extends 'base.html' %}

{% block title %}{{ route.name }} - 路线详情 - Talk&Walk{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>

    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:schedule_management' %}">
                        <i class="fa fa-calendar"></i> 行程安排
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>

        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h2>{{ route.name }}</h2>
                <div>
                    <a href="{% url 'routes:route_edit' route.id %}" class="btn btn-primary me-2">
                        <i class="fa fa-edit me-2"></i>编辑路线
                    </a>
                    <a href="{% url 'agency:schedule_management' %}" class="btn btn-outline-secondary">
                        <i class="fa fa-calendar me-2"></i>安排行程
                    </a>
                </div>
            </div>

            <div class="row">
                <div class="col-lg-8">
                    <div class="card mb-4">
                        <div class="card-header">
                            路线概览
                        </div>
                        <div class="card-body">
                            <div class="mb-3">
                                <strong>价格:</strong> ¥{{ route.price }}
                            </div>
                            <div class="mb-3">
                                <strong>描述:</strong>
                                <p>{{ route.description|linebreaks }}</p>
                            </div>
                            <div>
                                <strong>创建时间:</strong> {{ route.created_at|date:"Y-m-d H:i" }}
                            </div>
                        </div>
                    </div>

                    <div class="card">
                        <div class="card-header">
                            景点列表
                        </div>
                        <div class="card-body">
                            {% if spots %}
                            <div class="list-group">
                                {% for spot in spots %}
                                <div class="list-group-item list-group-item-action">
                                    <div class="d-flex justify-content-between align-items-center">
                                        <h5 class="mb-1">{{ forloop.counter }}. {{ spot.name }}</h5>
                                        <small>坐标: {{ spot.latitude }}, {{ spot.longitude }}</small>
                                    </div>
                                    <p class="mb-1">{{ spot.description|truncatechars:100 }}</p>
                                    {% if spot.images.all %}
                                    <small>有 {{ spot.images.count }} 张图片</small>
                                    {% else %}
                                    <small class="text-muted">暂无图片</small>
                                    {% endif %}
                                </div>
                                {% endfor %}
                            </div>
                            {% else %}
                            <div class="alert alert-info">
                                该路线暂无景点。点击"编辑路线"添加景点。
                            </div>
                            {% endif %}
                        </div>
                    </div>
                </div>

                <div class="col-lg-4">
                    <div class="card">
                        <div class="card-header">
                            相关行程
                        </div>
                        <div class="card-body">
                            {% if route.schedules.all %}
                            <div class="list-group">
                                {% for schedule in route.schedules.all %}
                                <a href="#" class="list-group-item list-group-item-action">
                                    <div class="d-flex w-100 justify-content-between">
                                        <h5 class="mb-1">{{ schedule.start_date|date:"Y-m-d" }}</h5>
                                        <small>{{ schedule.get_booking_count }}/{{ schedule.max_tourists }}</small>
                                    </div>
                                    <p class="mb-1">
                                        {% if schedule.guide %}
                                            导游: {{ schedule.guide.user.username }}
                                        {% else %}
                                            <span class="text-muted">未分配导游</span>
                                        {% endif %}
                                    </p>
                                    <small>价格: ¥{{ schedule.price }}</small>
                                </a>
                                {% endfor %}
                            </div>
                            {% else %}
                            <div class="alert alert-info">
                                该路线暂无相关行程安排。
                            </div>
                            <a href="{% url 'agency:schedule_management' %}" class="btn btn-outline-primary btn-sm">
                                <i class="fa fa-plus-circle me-2"></i>创建行程
                            </a>
                            {% endif %}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}

(17)talkwalk/templates/routes/route_form.html:
{% extends 'base.html' %}

{% block title %}{% if route %}编辑路线 - {{ route.name }}{% else %}创建路线{% endif %} - Talk&Walk{% endblock %}

{% block extra_css %}
<!-- 高德地图API -->
<link rel="stylesheet" href="https://a.amap.com/jsapi_demos/static/demo-center/css/demo-center.css" />
<style>
    #map-container {
        width: 100%;
        height: 500px;
        position: relative;
    }

    .spot-info-panel {
        max-height: 500px;
        overflow-y: auto;
    }

    .mode-controls {
        position: absolute;
        bottom: 10px;
        left: 10px;
        background: white;
        padding: 10px;
        border-radius: 5px;
        box-shadow: 0 2px 6px rgba(0,0,0,0.3);
        z-index: 100;
    }

    .coordinate-info {
        position: absolute;
        top: 10px;
        right: 10px;
        background: rgba(255,255,255,0.8);
        padding: 5px 10px;
        border-radius: 5px;
        font-size: 12px;
        z-index: 100;
    }

    .search-box {
        position: absolute;
        top: 10px;
        left: 10px;
        width: 300px;
        background: white;
        padding: 10px;
        border-radius: 5px;
        box-shadow: 0 2px 6px rgba(0,0,0,0.3);
        z-index: 100;
    }

    .spot-images {
        display: flex;
        flex-wrap: nowrap;
        overflow-x: auto;
        padding: 10px 0;
        gap: 10px;
    }

    .spot-image {
        width: 100px;
        height: 100px;
        object-fit: cover;
        border-radius: 5px;
        cursor: pointer;
    }

    .spot-image-container {
        position: relative;
    }

    .image-delete-btn {
        position: absolute;
        top: -5px;
        right: -5px;
        background: rgba(255,0,0,0.8);
        color: white;
        border: none;
        border-radius: 50%;
        width: 20px;
        height: 20px;
        font-size: 12px;
        line-height: 1;
        cursor: pointer;
    }
</style>
{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>

    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:schedule_management' %}">
                        <i class="fa fa-calendar"></i> 行程安排
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>

        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h2>{% if route %}编辑路线: {{ route.name }}{% else %}创建新路线{% endif %}</h2>
                <div>
                    {% if route %}
                    <button type="button" class="btn btn-success me-2" id="save-route-btn">
                        <i class="fa fa-save me-2"></i>保存路线
                    </button>
                    {% endif %}
                    <a href="{% if route %}{% url 'routes:route_detail' route.id %}{% else %}{% url 'agency:route_management' %}{% endif %}" class="btn btn-outline-secondary">
                        <i class="fa fa-arrow-left me-2"></i>返回
                    </a>
                </div>
            </div>

            <div class="row">
                <div class="col-lg-12 mb-4">
                    <div class="card">
                        <div class="card-header">
                            基本信息
                        </div>
                        <div class="card-body">
                            <form method="post" action="{% if route %}{% url 'routes:route_edit' route.id %}{% else %}{% url 'routes:route_create' %}{% endif %}" id="route-form">
                                {% csrf_token %}
                                <div class="row">
                                    <div class="col-md-6">
                                        <div class="mb-3">
                                            <label for="name" class="form-label">路线名称</label>
                                            <input type="text" class="form-control" id="name" name="name" value="{{ route.name|default:'' }}" required>
                                        </div>
                                    </div>
                                    <div class="col-md-6">
                                        <div class="mb-3">
                                            <label for="price" class="form-label">价格</label>
                                            <div class="input-group">
                                                <span class="input-group-text">¥</span>
                                                <input type="number" class="form-control" id="price" name="price" value="{{ route.price|default:'0' }}" min="0" step="0.01">
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="mb-3">
                                    <label for="description" class="form-label">路线描述</label>
                                    <textarea class="form-control" id="description" name="description" rows="3">{{ route.description|default:'' }}</textarea>
                                </div>
                                {% if not route %}
                                <div class="d-grid">
                                    <button type="submit" class="btn btn-primary">创建路线</button>
                                </div>
                                {% endif %}
                            </form>
                        </div>
                    </div>
                </div>
            </div>

            {% if route %}
            <div class="row">
                <div class="col-lg-8">
                    <div class="card">
                        <div class="card-header">
                            地图编辑器
                        </div>
                        <div class="card-body p-0">
                            <div id="map-container">
                                <div class="search-box">
                                    <div class="input-group">
                                        <input type="text" class="form-control" id="search-input" placeholder="输入地点搜索...">
                                        <button class="btn btn-outline-secondary" type="button" id="search-btn">搜索</button>
                                    </div>
                                </div>
                                <div class="coordinate-info" id="coordinate-info">坐标: 0.000000, 0.000000</div>
                                <div class="mode-controls">
                                    <div class="btn-group" role="group">
                                        <button type="button" class="btn btn-sm btn-outline-primary active" id="view-mode-btn">查看模式</button>
                                        <button type="button" class="btn btn-sm btn-outline-primary" id="edit-mode-btn">编辑模式</button>
                                        <button type="button" class="btn btn-sm btn-outline-primary" id="schedule-mode-btn">行程管理</button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="col-lg-4">
                    <div class="card">
                        <div class="card-header">
                            景点信息
                        </div>
                        <div class="card-body spot-info-panel" id="spot-info-panel">
                            <div class="text-center py-5" id="empty-spot-info">
                                <i class="fa fa-map-marker-alt fa-3x text-muted mb-3"></i>
                                <p class="text-muted">在地图上选择或添加景点查看详情</p>
                            </div>

                            <div id="spot-form-container" style="display: none;">
                                <form id="spot-form">
                                    <input type="hidden" id="spot-id" name="spot_id" value="">
                                    <input type="hidden" id="spot-latitude" name="latitude" value="">
                                    <input type="hidden" id="spot-longitude" name="longitude" value="">

                                    <div class="mb-3">
                                        <label for="spot-name" class="form-label">景点名称</label>
                                        <input type="text" class="form-control" id="spot-name" name="name" required>
                                    </div>

                                    <div class="mb-3">
                                        <label for="spot-description" class="form-label">景点描述</label>
                                        <textarea class="form-control" id="spot-description" name="description" rows="3"></textarea>
                                    </div>

                                    <div class="mb-3">
                                        <label class="form-label">坐标</label>
                                        <div class="input-group mb-2">
                                            <span class="input-group-text">纬度</span>
                                            <input type="text" class="form-control" id="display-latitude" readonly>
                                        </div>
                                        <div class="input-group">
                                            <span class="input-group-text">经度</span>
                                            <input type="text" class="form-control" id="display-longitude" readonly>
                                        </div>
                                    </div>

                                    <div class="mb-3">
                                        <label for="spot-images" class="form-label">景点图片</label>
                                        <input type="file" class="form-control" id="spot-images" name="images" multiple accept="image/*">
                                        <div class="spot-images mt-2" id="spot-images-preview"></div>
                                    </div>

                                    <div class="d-grid gap-2">
                                        <button type="button" class="btn btn-primary" id="save-spot-btn">保存景点</button>
                                        <button type="button" class="btn btn-danger" id="delete-spot-btn" style="display: none;">删除景点</button>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            {% endif %}
        </div>
    </div>
</div>
{% endblock %}

{% block extra_js %}
{% if route %}
<!-- 高德地图API -->
<script type="text/javascript" src="https://webapi.amap.com/maps?v=2.0&key={{ api_key }}"></script>
<script>
    // 地图功能
    let map;
    let markers = [];
    let polylines = [];
    let currentMode = 'view'; // 'view', 'edit', 'schedule'
    let spots = [];
    let selectedSpotId = null;

    // 初始化数据
    {% if spots %}
        {% for spot in spots %}
            spots.push({
                id: {{ spot.id }},
                name: "{{ spot.name|escapejs }}",
                description: "{{ spot.description|escapejs }}",
                latitude: {{ spot.latitude }},
                longitude: {{ spot.longitude }},
                order: {{ spot.order }},
                images: [
                    {% for image in spot.images.all %}
                        "{{ image.image.url }}",
                    {% endfor %}
                ]
            });
        {% endfor %}
    {% endif %}

    // 初始化地图
    function initMap() {
        map = new AMap.Map('map-container', {
            zoom: 11,
            resizeEnable: true
        });

        // 添加默认控件
        map.plugin(['AMap.ToolBar', 'AMap.Scale'], function(){
            map.addControl(new AMap.ToolBar());
            map.addControl(new AMap.Scale());
        });

        // 鼠标移动显示坐标
        map.on('mousemove', function(e) {
            document.getElementById('coordinate-info').innerText = `坐标: ${e.lnglat.getLat().toFixed(6)}, ${e.lnglat.getLng().toFixed(6)}`;
        });

        // 加载已有的景点
        loadSpots();

        // 编辑模式下，允许点击地图添加标记
        map.on('click', function(e) {
            if (currentMode === 'edit') {
                addMarkerAt(e.lnglat.getLng(), e.lnglat.getLat());
            }
        });
    }

    // 加载已有的景点
    function loadSpots() {
        clearMap();

        if (spots.length === 0) {
            return;
        }

        // 添加标记和连线
        spots.forEach((spot, index) => {
            const marker = new AMap.Marker({
                position: new AMap.LngLat(spot.longitude, spot.latitude),
                title: spot.name,
                extData: {
                    id: spot.id,
                    index: index
                }
            });

            marker.on('click', function() {
                selectSpot(spot.id);
            });

            markers.push(marker);

            // 如果有下一个点，添加连线
            if (index < spots.length - 1) {
                const nextSpot = spots[index + 1];
                const polyline = new AMap.Polyline({
                    path: [
                        new AMap.LngLat(spot.longitude, spot.latitude),
                        new AMap.LngLat(nextSpot.longitude, nextSpot.latitude)
                    ],
                    strokeColor: '#3498db',
                    strokeWeight: 5
                });

                polylines.push(polyline);
            }
        });

        // 将所有标记和线添加到地图
        map.add(markers);
        map.add(polylines);

        // 调整视图以包含所有标记
        if (markers.length > 0) {
            map.setFitView();
        }
    }

    // 清除地图上的标记和线
    function clearMap() {
        if (markers.length > 0) {
            map.remove(markers);
            markers = [];
        }

        if (polylines.length > 0) {
            map.remove(polylines);
            polylines = [];
        }
    }

    // 添加标记
    function addMarkerAt(lng, lat) {
        // 创建一个临时景点对象
        const newSpot = {
            id: null,  // 新景点ID为空，后端保存后会分配ID
            name: `景点${spots.length + 1}`,
            description: '',
            latitude: lat,
            longitude: lng,
            order: spots.length + 1,
            images: []
        };

        // 在地图上添加标记
        const marker = new AMap.Marker({
            position: new AMap.LngLat(lng, lat),
            title: newSpot.name,
            animation: 'AMAP_ANIMATION_DROP',
            extData: {
                id: null,
                index: spots.length
            }
        });

        marker.on('click', function() {
            showSpotForm(newSpot);
        });

        markers.push(marker);
        map.add(marker);

        // 如果已有其他景点，添加连线到上一个点
        if (spots.length > 0) {
            const lastSpot = spots[spots.length - 1];
            const polyline = new AMap.Polyline({
                path: [
                    new AMap.LngLat(lastSpot.longitude, lastSpot.latitude),
                    new AMap.LngLat(lng, lat)
                ],
                strokeColor: '#3498db',
                strokeWeight: 5
            });

            polylines.push(polyline);
            map.add(polyline);
        }

        // 显示表单
        showSpotForm(newSpot);
    }

    // 选择景点
    function selectSpot(spotId) {
        const spot = spots.find(s => s.id === spotId);
        if (spot) {
            selectedSpotId = spotId;
            showSpotForm(spot);
        }
    }

    // 显示景点表单
    function showSpotForm(spot) {
        document.getElementById('empty-spot-info').style.display = 'none';
        document.getElementById('spot-form-container').style.display = 'block';

        document.getElementById('spot-id').value = spot.id || '';
        document.getElementById('spot-name').value = spot.name;
        document.getElementById('spot-description').value = spot.description;
        document.getElementById('spot-latitude').value = spot.latitude;
        document.getElementById('spot-longitude').value = spot.longitude;
        document.getElementById('display-latitude').value = spot.latitude;
        document.getElementById('display-longitude').value = spot.longitude;

        // 显示已有图片
        const imagesContainer = document.getElementById('spot-images-preview');
        imagesContainer.innerHTML = '';

        spot.images.forEach(image => {
            const imgContainer = document.createElement('div');
            imgContainer.className = 'spot-image-container';

            const img = document.createElement('img');
            img.src = image;
            img.className = 'spot-image';
            img.onclick = function() {
                window.open(image, '_blank');
            };

            const deleteBtn = document.createElement('button');
            deleteBtn.className = 'image-delete-btn';
            deleteBtn.innerHTML = '×';
            deleteBtn.onclick = function(e) {
                e.preventDefault();
                // 实现删除图片功能
                if (confirm('确定删除这张图片？')) {
                    // 这里需要调用后端API删除图片
                    // 暂时简单处理为从DOM中移除
                    imgContainer.remove();
                }
            };

            imgContainer.appendChild(img);
            imgContainer.appendChild(deleteBtn);
            imagesContainer.appendChild(imgContainer);
        });

        // 显示或隐藏删除按钮
        if (spot.id) {
            document.getElementById('delete-spot-btn').style.display = 'block';
        } else {
            document.getElementById('delete-spot-btn').style.display = 'none';
        }
    }

    // 保存景点信息
    function saveSpotData() {
        const formData = new FormData(document.getElementById('spot-form'));
        const spotId = formData.get('spot_id');

        let url;
        if (spotId) {
            url = `/routes/spot/${spotId}/edit/`;
        } else {
            url = `/routes/{{ route.id }}/spot/create/`;
        }

        fetch(url, {
            method: 'POST',
            body: formData,
            headers: {
                'X-CSRFToken': '{{ csrf_token }}',
            },
        })
        .then(response => response.json())
        .then(data => {
            if (data.success) {
                alert('景点保存成功！');
                // 重新加载路线数据
                location.reload();
            } else {
                alert('保存失败，请重试。');
            }
        })
        .catch(error => {
            console.error('Error:', error);
            alert('发生错误，请重试。');
        });
    }

    // 删除景点
    function deleteSpot() {
        const spotId = document.getElementById('spot-id').value;

        if (!spotId) {
            return;
        }

        if (confirm('确定删除这个景点？此操作不可恢复。')) {
            fetch(`/routes/spot/${spotId}/delete/`, {
                method: 'POST',
                headers: {
                    'X-CSRFToken': '{{ csrf_token }}',
                    'Content-Type': 'application/json',
                },
            })
            .then(response => response.json())
            .then(data => {
                if (data.success) {
                    alert('景点删除成功！');
                    location.reload();
                } else {
                    alert('删除失败，请重试。');
                }
            })
            .catch(error => {
                console.error('Error:', error);
                alert('发生错误，请重试。');
            });
        }
    }

    // 保存整个路线
    function saveRoute() {
        const routeData = {
            route_id: {{ route.id }},
            route_name: document.getElementById('name').value,
            route_description: document.getElementById('description').value,
            price: document.getElementById('price').value,
        };

        fetch('/routes/{{ route.id }}/edit/', {
            method: 'POST',
            headers: {
                'X-CSRFToken': '{{ csrf_token }}',
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(routeData)
        })
        .then(response => response.json())
        .then(data => {
            if (data.success) {
                alert('路线保存成功！');
                window.location.href = `/routes/${data.route_id}/`;
            } else {
                alert('保存失败，请重试。');
            }
        })
        .catch(error => {
            console.error('Error:', error);
            alert('发生错误，请重试。');
        });
    }

    // 设置地图模式
    function setMapMode(mode) {
        currentMode = mode;

        // 更新按钮状态
        document.getElementById('view-mode-btn').classList.remove('active');
        document.getElementById('edit-mode-btn').classList.remove('active');
        document.getElementById('schedule-mode-btn').classList.remove('active');

        document.getElementById(`${mode}-mode-btn`).classList.add('active');

        // 根据模式设置不同的交互方式
        if (mode === 'view') {
            // 查看模式：禁用编辑功能
            map.setStatus({dragEnable: true, zoomEnable: true});
        } else if (mode === 'edit') {
            // 编辑模式：启用编辑功能
            map.setStatus({dragEnable: true, zoomEnable: true});
        } else if (mode === 'schedule') {
            // 行程管理模式
            map.setStatus({dragEnable: true, zoomEnable: true});
            // 跳转到行程安排页面
            window.location.href = '{% url "agency:schedule_management" %}';
        }
    }

    // 实现搜索功能
    function searchLocation() {
        const keyword = document.getElementById('search-input').value;
        if (!keyword) return;

        // 创建搜索服务
        AMap.plugin('AMap.PlaceSearch', function(){
            const placeSearch = new AMap.PlaceSearch({
                pageSize: 1,
                pageIndex: 1,
                city: '全国'
            });

            placeSearch.search(keyword, function(status, result) {
                if (status === 'complete' && result.info === 'OK') {
                    const poi = result.poiList.pois[0];
                    map.setCenter([poi.location.lng, poi.location.lat]);
                    map.setZoom(14);
                } else {
                    alert('未找到相关位置');
                }
            });
        });
    }

    // 事件监听
    document.addEventListener('DOMContentLoaded', function() {
        // 初始化地图
        initMap();

        // 监听模式切换
        document.getElementById('view-mode-btn').addEventListener('click', function() {
            setMapMode('view');
        });

        document.getElementById('edit-mode-btn').addEventListener('click', function() {
            setMapMode('edit');
        });

        document.getElementById('schedule-mode-btn').addEventListener('click', function() {
            setMapMode('schedule');
        });

        // 保存景点
        document.getElementById('save-spot-btn').addEventListener('click', function() {
            saveSpotData();
        });

        // 删除景点
        document.getElementById('delete-spot-btn').addEventListener('click', function() {
            deleteSpot();
        });

        // 保存路线
        document.getElementById('save-route-btn').addEventListener('click', function() {
            saveRoute();
        });

        // 搜索位置
        document.getElementById('search-btn').addEventListener('click', function() {
            searchLocation();
        });

        document.getElementById('search-input').addEventListener('keyup', function(e) {
            if (e.key === 'Enter') {
                searchLocation();
            }
        });

        // 图片预览
        document.getElementById('spot-images').addEventListener('change', function() {
            const files = this.files;
            const previewContainer = document.getElementById('spot-images-preview');

            for (let i = 0; i < files.length; i++) {
                const file = files[i];
                const reader = new FileReader();

                reader.onload = function(e) {
                    const imgContainer = document.createElement('div');
                    imgContainer.className = 'spot-image-container';

                    const img = document.createElement('img');
                    img.src = e.target.result;
                    img.className = 'spot-image';

                    imgContainer.appendChild(img);
                    previewContainer.appendChild(imgContainer);
                }

                reader.readAsDataURL(file);
            }
        });
    });
</script>
{% endif %}
{% endblock %}

(18)talkwalk/templates/routes/route_list.html:
<!-- talkwalk/templates/routes/route_list.html -->
{% extends 'base.html' %}

{% block title %}路线列表 - Talk&Walk{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <!-- 顶部导航栏 -->
        <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
            <div class="container-fluid">
                <a class="navbar-brand" href="{% url 'agency:dashboard' %}">Talk&Walk</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                                {% if user.profile_pic %}
                                    <img src="{{ user.profile_pic.url }}" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% else %}
                                    <img src="https://via.placeholder.com/40" alt="{{ user.username }}" class="avatar" id="nav-profile-pic">
                                {% endif %}
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end profile-dropdown">
                                <li><h6 class="dropdown-header">{{ agency.agency_name }}</h6></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="{% url 'profile' %}"><i class="fa fa-user me-2"></i>个人设置</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}"><i class="fa fa-sign-out-alt me-2"></i>退出登录</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>

    <div class="row mt-3">
        <!-- 侧边栏 -->
        <div class="col-lg-2 sidebar">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:dashboard' %}">
                        <i class="fa fa-dashboard"></i> 总览
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link active" href="{% url 'agency:route_management' %}">
                        <i class="fa fa-map"></i> 路线管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:guide_list' %}">
                        <i class="fa fa-user-circle"></i> 导游管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:booking_management' %}">
                        <i class="fa fa-calendar-check"></i> 预订管理
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:schedule_management' %}">
                        <i class="fa fa-calendar"></i> 行程安排
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:notifications' %}">
                        <i class="fa fa-bell"></i> 通知
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'agency:analytics' %}">
                        <i class="fa fa-chart-bar"></i> 统计分析
                    </a>
                </li>
            </ul>
        </div>

        <!-- 主内容区 -->
        <div class="col-lg-10 content">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h2>路线管理</h2>
                <a href="{% url 'routes:route_create' %}" class="btn btn-primary">
                    <i class="fa fa-plus me-2"></i>创建新路线
                </a>
            </div>

            <div class="row">
                {% for route in routes %}
                <div class="col-lg-4 mb-4">
                    <div class="card h-100">
                        <div class="card-body">
                            <h5 class="card-title">{{ route.name }}</h5>
                            <p class="card-text text-muted">
                                <small>创建于 {{ route.created_at|date:"Y-m-d" }}</small>
                            </p>
                            <p class="card-text">
                                {% if route.description %}
                                    {{ route.description|truncatechars:100 }}
                                {% else %}
                                    <span class="text-muted">暂无描述</span>
                                {% endif %}
                            </p>
                        </div>
                        <div class="card-footer bg-white">
                            <div class="d-flex justify-content-between">
                                <span>
                                    <strong>价格:</strong> ¥{{ route.price }}
                                </span>
                                <div>
                                    <a href="{% url 'routes:route_detail' route.id %}" class="btn btn-sm btn-outline-primary me-2">
                                        <i class="fa fa-eye"></i> 查看
                                    </a>
                                    <a href="{% url 'routes:route_edit' route.id %}" class="btn btn-sm btn-outline-secondary">
                                        <i class="fa fa-edit"></i> 编辑
                                    </a>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                {% empty %}
                <div class="col-12">
                    <div class="alert alert-info text-center">
                        <i class="fa fa-info-circle fa-2x mb-3"></i>
                        <p>您还没有创建任何旅游路线。</p>
                        <p>点击上方的"创建新路线"按钮开始创建您的第一条路线。</p>
                    </div>
                </div>
                {% endfor %}
            </div>
        </div>
    </div>
</div>
{% endblock %}




		